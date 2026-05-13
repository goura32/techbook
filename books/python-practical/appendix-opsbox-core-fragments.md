# Appendix C. OpsBox の代表断片

## この付録の役割

付録 A は `OpsBox` の構造、付録 B は型と関数の対応を整理するためのものでした。この付録 C では、本文で断片的に登場したコードを、もう少し連続した形で確認できるようにします。

ただし、ここに載せるのはアプリケーション全体の完全実装ではありません。本書の主張に直接関係する「境界」「変換」「設定」「CLI」「非同期」が見える最小限の代表断片に絞ります。

## 想定構成

```text
opsbox/
  src/
    opsbox/
      cli.py
      config.py
      models.py
      async_sync.py
      clients/
        status_api.py
      services/
        reporting.py
```

## 1. `models.py` の代表断片

```python
from dataclasses import dataclass
from datetime import datetime
from typing import Literal

JobStatus = Literal["queued", "running", "failed", "finished"]


@dataclass(slots=True)
class Job:
    id: str
    project: str
    status: JobStatus
    started_at: datetime | None
    finished_at: datetime | None
    owner_name: str | None
```

ここでは、外部 API の事情はまだ登場しません。`models.py` はあくまで内部で信頼する型の土台です。

## 2. `clients/status_api.py` の代表断片

```python
from datetime import datetime
from typing import TypedDict

from opsbox.models import Job


class JobPayload(TypedDict):
    id: str
    project: str
    status: str
    started_at: str | None
    finished_at: str | None
    owner_name: str | None


def to_job(payload: JobPayload) -> Job:
    return Job(
        id=payload["id"],
        project=payload["project"],
        status=_parse_status(payload["status"]),
        started_at=_parse_datetime(payload["started_at"]),
        finished_at=_parse_datetime(payload["finished_at"]),
        owner_name=payload["owner_name"],
    )


def _parse_datetime(value: str | None) -> datetime | None:
    if value is None:
        return None
    return datetime.fromisoformat(value)
```

この断片で重要なのは、外部レスポンス辞書と内部 dataclass を分けて持っていることです。

## 3. `config.py` の代表断片

```python
from dataclasses import dataclass
import os
import tomllib
from pathlib import Path


@dataclass(slots=True)
class OpsBoxSettings:
    api_base_url: str
    api_token: str
    default_project: str | None


def load_settings(config_path: Path | None = None) -> OpsBoxSettings:
    file_values: dict[str, object] = {}
    if config_path and config_path.exists():
        file_values = tomllib.loads(config_path.read_text(encoding="utf-8"))

    api_base_url = os.environ.get("OPSBOX_API_BASE_URL") or file_values.get("api_base_url")
    api_token = os.environ.get("OPSBOX_API_TOKEN") or file_values.get("api_token")
    default_project = os.environ.get("OPSBOX_DEFAULT_PROJECT") or file_values.get("default_project")

    if not api_base_url or not api_token:
        raise ValueError("API settings are required")

    return OpsBoxSettings(
        api_base_url=str(api_base_url),
        api_token=str(api_token),
        default_project=str(default_project) if default_project else None,
    )
```

環境変数も設定ファイルも、ここでは外部入力です。`load_settings()` でいったんまとめてから先へ渡します。

## 4. `services/reporting.py` と `cli.py` の代表断片

```python
from dataclasses import dataclass

from opsbox.models import Job


@dataclass(slots=True)
class DailyReport:
    total: int
    failed: int
    running: int


def build_daily_report(jobs: list[Job]) -> DailyReport:
    return DailyReport(
        total=len(jobs),
        failed=sum(1 for job in jobs if job.status == "failed"),
        running=sum(1 for job in jobs if job.status == "running"),
    )
```

```python
import argparse
import sys

from opsbox.clients.status_api import fetch_jobs
from opsbox.config import load_settings
from opsbox.services.reporting import build_daily_report


def main(argv: list[str] | None = None) -> int:
    parser = argparse.ArgumentParser()
    parser.add_argument("--project")
    parser.add_argument("--config")
    args = parser.parse_args(argv)

    settings = load_settings(args.config)
    jobs = fetch_jobs(settings, project=args.project or settings.default_project)
    report = build_daily_report(jobs)
    print(f"総ジョブ数: {report.total}")
    print(f"失敗: {report.failed}")
    print(f"実行中: {report.running}")
    return 0


if __name__ == "__main__":
    raise SystemExit(main(sys.argv[1:]))
```

CLI は外部 API の生レスポンスを直接知らず、`Job` と `DailyReport` を扱います。これが、入口の責務を軽くするポイントです。

## 5. `async_sync.py` の代表断片

```python
import asyncio

from opsbox.clients.status_api import fetch_jobs_async
from opsbox.config import OpsBoxSettings


async def sync_projects(settings: OpsBoxSettings, projects: list[str]) -> dict[str, int]:
    async with asyncio.TaskGroup() as group:
        tasks = {
            project: group.create_task(fetch_jobs_async(settings, project))
            for project in projects
        }

    return {project: len(task.result()) for project, task in tasks.items()}
```

ここで見てほしいのは、非同期導入の範囲を「複数 API 呼び出しの待ち時間」に絞っていることです。内部モデルや出力まで async 化するのではなく、必要なところだけへ限定しています。

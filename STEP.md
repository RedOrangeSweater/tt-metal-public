# Шаг 00 — архив текущего shutovilyaep/tt-metal main

## 1. Цель

Зафиксировать текущий `main` до force-with-lease, чтобы был rollback reference.

## 2. Где выполнять

Терминал на машине с push-доступом к `shutovilyaep/tt-metal`.

## 3. Команды

```bash
set -euo pipefail
WORKDIR=~/shutov-release-2026-07
mkdir -p "$WORKDIR"
cd "$WORKDIR"

rm -rf tt-metal
git clone git@github.com:shutovilyaep/tt-metal.git
cd tt-metal
git fetch origin

EXPECTED_OLD=63ca6ee0ab221360c161d426f865ffc70d1b6362
ACTUAL_OLD=$(git rev-parse origin/main)
echo "origin/main=$ACTUAL_OLD"
echo "$ACTUAL_OLD" | tee ../metal-old-main.sha

test "$ACTUAL_OLD" = "$EXPECTED_OLD"

ARCHIVE=archive/pre-shutov-release-202607
git push origin "origin/main:refs/heads/${ARCHIVE}"
git ls-remote --heads origin "$ARCHIVE"

git tag -a "pre-shutov-release-202607" "$ACTUAL_OLD" -m "archive before shutov tip force-with-lease"
git push origin "pre-shutov-release-202607"
```

## 4. Ожидаемый результат

- `origin/main` = `63ca6ee0ab221360c161d426f865ffc70d1b6362` (или осознанно обновлённый baseline после STOP/ревью).
- Remote branch `archive/pre-shutov-release-202607` указывает на тот же SHA.
- Tag `pre-shutov-release-202607` виден на GitHub.

## 5. STOP

- `origin/main` ≠ `63ca6ee0…` → не пушь tip. Запиши новый SHA, сравни с expected tip `96e4b712…`, реши вручную.
- Push archive branch/tag не прошёл → не иди к force-with-lease.

## 6. Записать в RELEASE_LOG.md

- timestamp, `ACTUAL_OLD`, URL ветки archive, tag name

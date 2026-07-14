# Шаг 40 — PyPI Trusted Publisher для ttnn-shutov

## 1. Цель

Привязать GitHub Actions OIDC к project `ttnn-shutov`. API token в Secrets для prod YAML не подходит (job не передаёт `password`).

## 2. Где выполнять

https://pypi.org/manage/project/ttnn-shutov/settings/publishing/

## 3. Нажатия

1. Project `ttnn-shutov` → Managing → Publishing
2. Add a new publisher (GitHub)
3. Поля **ровно**:
   - Owner: `shutovilyaep`
   - Repository name: `tt-metal`
   - Workflow name: `release-ttnn-shutov-from-source.yaml`
   - Environment name: `pypi`
4. Add / Save и перечитай четыре поля

## 4. Ожидаемый результат

Pending или Active Trusted Publisher с полями выше.

## 5. STOP

Любое поле отличается; publisher от другого repo. Не запускай `publish_target=pypi`.

## 6. Записать в RELEASE_LOG.md

Копия четырёх полей, status Pending/Active

# pipelines-rust

<img width="1391" height="293" alt="image" src="https://github.com/user-attachments/assets/2019ed0f-10d1-4257-945a-e7c58a51c3c5" />


# 🦀 pipelines-rust

Учебный проект: **CI-пайплайн на GitHub Actions** для Rust-приложения.

---

## 📖 О проекте

Простое Rust-приложение (`main`), для которого настроен CI-пайплайн.
При каждом `push` и `pull request` в ветки `main` / `master` автоматически:

- 🧹 проверяется форматирование через **rustfmt**
- 🧪 прогоняется линтер **clippy** (с `-D warnings`)
- 🏗️ собирается debug и release билд
- ✅ запускаются тесты через `cargo test`
- 🐳 собирается **Docker-образ** (multi-stage: rust → debian-slim) и сохраняется как артефакт

---

## 📂 Структура проекта

```
.
├── .github/
│   └── workflows/
│       └── rust-ci.yml     # GitHub Actions workflow
├── src/
│   └── main.rs             # основной код
├── Cargo.toml              # манифест проекта
├── Cargo.lock              # lock-файл зависимостей
├── Dockerfile              # multi-stage: rust → debian-slim
├── .dockerignore
├── .gitignore
└── README.md
```

---

## ⚙️ Как работает пайплайн

| Job | Шаг | Что делает |
|-----|-----|-----------|
| **lint** | Format | `cargo fmt --all -- --check` |
| | Clippy | `cargo clippy -- -D warnings` |
| **test** | Check | `cargo check` |
| | Build | `cargo build` (debug + release) |
| | Test | `cargo test` |
| **docker-build** | Buildx | Сборка образа с кэшем GHA |
| | Save + Upload | Сохраняет образ как артефакт (`docker-image.tar.gz`) |
| | Test | `docker run --rm my-rust-app:latest` |

---

## 🐳 Проверка локально

### Сборка образа

```bash
docker build -t my-rust-app:latest .
```

Проверить, что образ создан:

```bash
docker images | grep my-rust-app
```

### Запуск контейнера

```bash
docker run --rm my-rust-app:latest
```

Ожидаемый вывод:

```
Hello from Rust in Docker! 🦀
```
<img width="574" height="80" alt="image" src="https://github.com/user-attachments/assets/d09e8a84-5789-4fc6-8bc3-57f122404053" />

Зайти внутрь контейнера:

```bash
docker run -it --rm --entrypoint /bin/bash my-rust-app:latest
```

Выход:

```bash
exit
```

---

## 🧪 Проверка кода локально

```bash
cargo fmt --all -- --check
cargo clippy -- -D warnings
cargo build --release
cargo test
```

---

## 🏗️ Стек

- **Rust (edition 2021)**
- **Cargo** — сборка
- **rustfmt** — форматирование
- **clippy** — линтинг
- **Docker** — multi-stage сборка (rust → debian-slim)
- **GitHub Actions** — CI

---

## 📄 Лицензия

MIT — свободно для учебных целей.

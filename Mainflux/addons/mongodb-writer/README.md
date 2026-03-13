# MongoDB Writer Addon Setup

This folder contains the Mainflux MongoDB writer addon config and local environment files.

## Files

- `docker-compose.yml`: Addon services and env wiring.
- `config.toml`: Writer subscriber/transformer config.
- `.env`: Local secrets and runtime variables (ignored by git).
- `.env.example`: Safe template without real secrets.
- `.vscode/extensions.json`: Recommends MongoDB VS Code extension.

## 1. Configure Atlas URI

Edit `.env` and set:

```env
MF_MONGO_WRITER_DB_HOST=mongodb+srv://<username>:<url_encoded_password>@cluster0.55mgzn.mongodb.net/
MF_MONGO_WRITER_DB_PORT=27017
```

Rules:
- Do not wrap password with `< >` in `.env` values.
- URL-encode special characters in password (`@`, `:`, `/`, `#`, etc.).

## 2. Connect From VS Code

1. Install extension: `MongoDB for VS Code` (`mongodb.mongodb-vscode`).
2. Open Command Palette.
3. Run `MongoDB: Connect`.
4. Paste the URI from `.env` (`MF_MONGO_WRITER_DB_HOST`).

## 3. Quick Connection Test (Terminal)

From this folder:

```bash
set -a && source .env && set +a
python3 - <<'PY'
import os
from pymongo import MongoClient
uri = os.environ['MF_MONGO_WRITER_DB_HOST']
print(MongoClient(uri, serverSelectionTimeoutMS=10000).admin.command('ping'))
PY
```

If you see `AuthenticationFailed`, verify Atlas DB user/password and network access rules.

## Security Notes

- Never commit `.env`.
- Commit only `.env.example`.
- Rotate credentials if they were ever shared in chat or committed by mistake.

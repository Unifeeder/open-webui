# How To Run Locally

This is a minimal local run guide based on `runme.md`, without secrets.

## Terminals
Use two terminals: one at repo root and one in `backend/`.

## Frontend
```bash
npm run dev
```

## Backend
```bash
conda activate open-webui
export WEBUI_CHAT_ENCRYPTION_KEY="<key_insert_here>"
export WEBUI_CHAT_ENCRYPT_OLD_CHATS=true
cd backend
sh dev.sh
```

## Unit Tests
```bash
conda activate open-webui
python -m pytest backend/open_webui/test/util/test_chat_encryption.py
python -m pytest backend/open_webui/test/util/test_encrypt_old_chats.py
python -m pytest backend/open_webui/test/apps/webui/models/test_chats_encryption.py
```

## Notes
- Remember to start the Ollama app if your setup requires it.
- If you track DB state manually, confirm changes in your DB admin tool.

# Additions

This addition adds user chat encryption support, such that cahts are not stored as plaintext in the database.

## Behavior
- Encrypts chat content in db at rest when `WEBUI_CHAT_ENCRYPTION_KEY` is set.
- Optionally schedules encryption of old chats on user login when `WEBUI_CHAT_ENCRYPT_OLD_CHATS=true`.
- Encryption is idempotent and only applies to plaintext content.

## Backend Changes
- Modified:
	- `backend/open_webui/routers/chats.py`
	- `backend/open_webui/models/chats.py`
	- `backend/open_webui/env.py`
	- `backend/open_webui/main.py`
	- `backend/open_webui/routers/auths.py`
- Added:
	- `backend/open_webui/utils/db/chat_encryption.py`
	- `backend/open_webui/utils/db/encrypt_old_chats.py`
	- `backend/open_webui/test/util/test_chat_encryption.py`
	- `backend/open_webui/test/util/test_encrypt_old_chats.py`
	- `backend/open_webui/test/apps/webui/models/test_chats_encryption.py`
- Notes:
	- `backend/open_webui/utils/db/chat_encryption.py`: centralized encrypt/decrypt helpers.
	- `backend/open_webui/utils/db/encrypt_old_chats.py`: batch encryption for historical chats.
	- `backend/open_webui/models/chats.py`: encrypt on write, normalize on read to return plaintext to UI.
	- `backend/open_webui/routers/chats.py`: normalize chat payloads before returning.
	- `backend/open_webui/routers/auths.py`: schedule old-chat encryption on signin.

## Tests
- Test files:
	- `backend/open_webui/test/util/test_chat_encryption.py`
	- `backend/open_webui/test/util/test_encrypt_old_chats.py`
	- `backend/open_webui/test/apps/webui/models/test_chats_encryption.py`

### Unit Tests
```bash
python -m pytest backend/open_webui/test/util/test_chat_encryption.py
python -m pytest backend/open_webui/test/util/test_encrypt_old_chats.py
python -m pytest backend/open_webui/test/apps/webui/models/test_chats_encryption.py
```

### Unit Test Results
```text
python -m pytest backend/open_webui/test/util/test_chat_encryption.py
==================================================== test session starts =====================================================
platform win32 -- Python 3.11.14, pytest-8.4.2, pluggy-1.6.0
configfile: pyproject.toml
plugins: anyio-4.12.1, langsmith-0.6.7, docker-3.2.5
collected 2 items
backend\open_webui\test\util\test_chat_encryption.py ..                                                                 [100%]
===================================================== 2 passed in 6.69s ======================================================

python -m pytest backend/open_webui/test/util/test_encrypt_old_chats.py
====================================================== warnings summary ======================================================
platform win32 -- Python 3.11.14, pytest-8.4.2, pluggy-1.6.0
configfile: pyproject.toml
plugins: anyio-4.12.1, langsmith-0.6.7, docker-3.2.5
collected 1 item
backend\open_webui\test\util\test_encrypt_old_chats.py .                                                                [100%]
================================================ 1 passed, 1 warning in 8.69s ================================================

python -m pytest backend/open_webui/test/apps/webui/models/test_chats_encryption.py
==================================================== test session starts =====================================================
platform win32 -- Python 3.11.14, pytest-8.4.2, pluggy-1.6.0
configfile: pyproject.toml
plugins: anyio-4.12.1, langsmith-0.6.7, docker-3.2.5
collected 1 item
backend\open_webui\test\apps\webui\models\test_chats_encryption.py .                                                    [100%]
===================================================== 1 passed in 7.23s ======================================================
```

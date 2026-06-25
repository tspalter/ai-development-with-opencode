# AGENTS.md

Django 5.2.3 tutorial project (server-rendered Bootstrap templates, SQLite) for learning AI-assisted development with OpenCode.

## Layout (read first — it's confusing)

Two nested folders share the name `MyProjects`:
- Repo root `ai-development-with-opencode/` — only `README.md` and `.gitattributes` are committed.
- `MyProjects/` — Django project root. **Run every `manage.py` command from here** (`manage.py` and `db.sqlite3` live here).
- `MyProjects/MyProjects/` — settings package. Settings module is `MyProjects.settings`; root URLconf is `MyProjects.urls`.

Apps:
- `blog/` — `Post` model; views: `index`, `dashboard` (login-required), `contact`, `search`.
- `accounts/` — `Profile` model (1:1 with `auth.User`); views: `signup`, `login`, `logout`, `profile`, `profile/edit`.

## Setup (nothing runs out of the box)

- No `requirements.txt` / `pyproject.toml` / lockfile and no virtualenv committed.
- Django is **not installed** in the active interpreter. Install before any `manage.py` command:
  `python -m pip install "django==5.2.3"`
- Default `python` here is 3.14; Django 5.2 officially targets Python 3.10–3.13. Use `py -3.13` (also installed) if you hit install/runtime issues.

## Commands (from `MyProjects/`)

- Run: `python manage.py runserver`
- Migrate: `python manage.py makemigrations` then `python manage.py migrate`
- Test: `python manage.py test` (single app: `python manage.py test blog`). No real tests yet — `tests.py` files are stubs.
- Superuser: `python manage.py createsuperuser`

## Project-specific gotchas

- **No `.gitignore`, and all of `MyProjects/` is currently untracked.** Do NOT stage `db.sqlite3`, `__pycache__/`, or the Windows `desktop.ini` files (present in every dir) when committing.
- A `Profile` is auto-created for every new `User` by a `post_save` signal in `accounts/models.py` — never create profiles manually.
- Logout is intentionally GET-allowed via `LogoutAllowGetView` in `accounts/urls.py` (the navbar logout is a plain link). Preserve this when editing auth URLs.
- `settings.py` hardcodes `SECRET_KEY` and `DEBUG = True` — dev-only, not production config.
- Templates extend `templates/base.html` (Bootstrap 5 via CDN); app templates are namespaced under `<app>/templates/<app>/`.

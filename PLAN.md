# Project Plan — MyProjects (Django teaching demo)

Single source of truth for planning and execution. Scope: **teaching/tutorial demo**, **multi-author blog**. Each task is intended to be a small, readable, self-contained change.

Last updated: 2026-06-24

## How to use this file
- Update each phase's **Status** as work progresses.
- Tick task checkboxes: `[ ]` = todo, `[x]` = done.
- Keep the summary table in sync with the phase sections.

**Status values:** `Not started` · `In progress` · `Blocked` · `Done`

## Progress overview

| Phase | Focus | Status |
|-------|-------------------------------------|-------------|
| 0 | Correctness & safety fixes | Not started |
| 1 | Make the blog real (multi-author) | Not started |
| 2 | Authoring & accounts | Not started |
| 3 | Light quality (tests, deps) | Not started |
| — | Cleanups (fold in as touched) | Not started |

---

## Phase 0 — Correctness & safety fixes
**Status:** Not started
**Goal:** Remove broken/leaky behavior with minimal, teachable changes. Ships as one small PR.

- [ ] Render messages in `templates/base.html` (`{% if messages %}` block) so signup/contact feedback appears
- [ ] Fix `/search/`: filter `is_public=True` and stop listing all posts on an empty query (`blog/views.py`)
- [ ] Add `@login_required` to `edit_profile_view` (`accounts/views.py`)
- [ ] Add `.gitignore` (`db.sqlite3`, `__pycache__/`, `desktop.ini`, venv)
- [ ] (Optional) Externalize `SECRET_KEY` / `DEBUG` / `ALLOWED_HOSTS` via environment

## Phase 1 — Make the blog real (multi-author core)
**Status:** Not started
**Goal:** Readers can browse and read posts; posts are owned by users. This is the headline milestone.

- [ ] Public, paginated **post list** on `/` + template
- [ ] **Post detail** at `/post/<slug>/`; wire the dead links; increment `views`
- [ ] Convert `Post.author` to `ForeignKey(User)` with a data migration (backfill existing rows)
- [ ] Auto-set `author = request.user` on post creation
- [ ] **Dashboard** shows "my posts" filtered by `request.user`

## Phase 2 — Authoring & accounts
**Status:** Not started
**Goal:** Users can author content and manage their accounts.

- [ ] Post **create / edit / delete**, login-required and owner-scoped (`ModelForm`)
- [ ] **Password reset** flow using Django's built-in views + console email backend
- [ ] (Optional) Persist contact submissions as a `ContactMessage` model
- [ ] (Optional) Enrich `Profile` (avatar / birthdate fields already stubbed)

## Phase 3 — Light quality (demo-appropriate)
**Status:** Not started
**Goal:** Demonstrate testing and reproducible setup; illustrative, not exhaustive.

- [ ] Example tests: auth flow, search privacy, ownership permissions (`tests.py` are empty today)
- [ ] `requirements.txt` pinning `django==5.2.3`

---

## Cleanups (fold in as the relevant area is touched)
**Status:** Not started

- [ ] Remove orphaned `contact_thanks` route + `blog/templates/blog/thanks.html`
- [ ] Remove empty `accounts/templates/accounts/logout.html`
- [ ] Remove commented code in `accounts/models.py`
- [ ] De-duplicate admin slug logic (`prepopulated_fields` vs `save_model`) in `blog/admin.py`

## Out of scope (given teaching-demo intent)
Deliberately deferred; not building now:
- Settings split (base/dev/prod)
- WhiteNoise / static pipeline / `collectstatic`
- CI and production security headers (HSTS, secure cookies)
- Secret-manager configuration

## Notes
- Phase 0 items are independent and make a clean first PR.
- Phase 1 items (list, detail, author FK, dashboard) should land together as the "it's actually a blog now" milestone.

## Plan: Add backend FastAPI tests

TL;DR: Add a new top-level `tests/` directory with FastAPI backend tests, make `src` importable as a package, and verify with `pytest`.

**Steps**
1. Create a new `tests/` directory at the repository root.
2. Add `src/__init__.py` so the existing `src/app.py` can be imported as `from src.app import app` in tests.
3. Add `tests/conftest.py` with a `TestClient` fixture for the FastAPI app and a fixture to snapshot and restore the `activities` global between tests.
4. Add `tests/test_app.py` with targeted backend API tests that each follow Arrange-Act-Assert structure:
   - root path redirects to `/static/index.html`
   - `GET /activities` returns the activity list with status 200
   - `POST /activities/{activity_name}/signup` successfully signs up a student
   - duplicate signup returns status 400
   - signup for a missing activity returns status 404
   - `DELETE /activities/{activity_name}/signup` unregisters a student successfully
   - unregistering a non-signed-up student returns status 400
5. Add `pytest` to `requirements.txt` so the test dependency is installed with the project.
6. Use `from src.app import app, activities` in test code and keep the existing `pytest.ini` configuration.
7. Run `pytest` from the repository root to confirm tests execute and pass.

**Relevant files**
- `/workspaces/copilotfundamentals/src/app.py` — existing FastAPI app under test
- `/workspaces/copilotfundamentals/pytest.ini` — pytest config to keep
- `/workspaces/copilotfundamentals/requirements.txt` — add pytest dependency
- `/workspaces/copilotfundamentals/src/__init__.py` — new package marker
- `/workspaces/copilotfundamentals/tests/conftest.py` — new fixtures
- `/workspaces/copilotfundamentals/tests/test_app.py` — new backend API tests

**Verification**
1. Run `pytest` from the repository root and confirm all tests pass.
2. Confirm the new `tests/` directory exists and contains `conftest.py` and `test_app.py`.
3. Verify `src/__init__.py` exists so the app can be imported consistently from tests.
4. Confirm state-reset fixture avoids cross-test pollution from the mutable `activities` global.

**Decisions**
- Use `fastapi.testclient.TestClient` for backend tests.
- Keep tests focused on backend API behavior only, not frontend static assets.
- Add a package marker in `src` rather than changing the app structure.

# Repository Guidelines

## Project Structure & Module Organization
- `agents/` hosts MetaGPT roles/actions plus `agents/tests/` for behavior checks.
- `app.py` and `web_demo-*.py` launch Streamlit/Gradio demos; UI assets live in `assets/`, front/back experiments sit in `front/` and `back/`.
- `model/` stores downloaded checkpoints (gitignored) and `xtuner_config/` tracks fine-tuning recipes; keep new configs self-documented.
- Data, RAG, and automation utilities live under `datasets/`, `generate_data/`, `rag/`, `scripts/`, and deployment guidance resides in `deploy/` + `quick_start/`.

## Build, Test, and Development Commands
- `python3 -m venv .venv && source .venv/bin/activate` — create an isolated environment.
- `pip install -r requirements.txt` — install transformers, Streamlit, MetaGPT fork, etc.
- `python download_model.py <openxlab_repo>` — fetch weights into `model/<name>` (e.g., `python download_model.py ajupyter/EmoLLM_aiwei`).
- `python app.py` — downloads the selected model and launches the default Streamlit UI on `http://0.0.0.0:7860`.
- `streamlit run web_demo-aiwei.py --server.address=0.0.0.0 --server.port 7860` — iterate on a single demo without changing `app.py`.

## Coding Style & Naming Conventions
- Target PEP 8: 4-space indents, `snake_case` for functions/modules, `PascalCase` for Roles/Actions, constants in `UPPER_SNAKE`.
- Type-hint public APIs, add docstrings explaining mental-health behaviors, and favor pure helpers in `agents/` or `scripts/` instead of bloating entry points.
- Use f-strings plus `metagpt.logs.logger` for observability and keep user-facing copy in localized markdown files (`README*.md`) when possible.

## Testing Guidelines
- Pytest backs the suite; create mirrors of source paths under `agents/tests/` using `test_<feature>.py`.
- Each new agent/action should assert Markdown or dialogue outputs with deterministic seeds/fixtures.
- Run `python -m pytest agents/tests -k <feature>` locally and note limitations (e.g., external API reliance) in the PR if tests must be skipped.

## Commit & Pull Request Guidelines
- Follow the existing concise imperative style (`add cite`, `update readme`); include scope prefixes when helpful (`agents: refine writer`) and keep to ≤72 characters.
- Reference linked work via `Fixes #123` or `(#123)` to match merge history.
- PRs need a clear summary, reproduction/launch commands, screenshots of UI changes, and confirmation that large weights or personal data stay out of the diff.

## Security & Configuration Tips
- Never commit downloaded checkpoints, API tokens, or end-user transcripts; `.gitignore` assumes `model/` stays local.
- Document required environment variables (e.g., `OPENXLAB_AUTH_TOKEN`) in PR descriptions but share secrets through your platform’s secret store, not the repo.

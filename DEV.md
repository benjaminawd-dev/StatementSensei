# Dev fork

This fork is a staging copy of [benjamin-awd/StatementSensei](https://github.com/benjamin-awd/StatementSensei),
used to try changes on Streamlit Community Cloud before they reach production.

- Dev app: https://statementsensei-dev.streamlit.app (deploys from this repo's `main`)
- Production app: https://statementsensei.streamlit.app

## Testing branches
`main` here is reset to upstream `main` for each test run, so nothing else is kept on it.
This file lives on the `dev-notes` branch and is merged back in each time.

```sh
git remote add dev https://github.com/benjaminawd-dev/StatementSensei
git fetch origin && git fetch dev
git checkout -B dev-test origin/main
git merge --no-ff dev/dev-notes   # keeps this file
git merge --no-ff <branch>        # repeat for each branch to test
git push --force dev dev-test:main
```

Once the app has redeployed, run the smoke test against it:

```sh
uv run --no-project --with playwright playwright install chromium
uv run --no-project --with playwright python .github/scripts/smoke_test.py \
  --app-url https://statementsensei-dev.streamlit.app --expected-version v<version>
```

## What runs when `main` is pushed
- `release-please` opens a release PR here, which can be closed.
- `docker-latest` fails at Docker Hub login because this fork has no Docker Hub secrets, so test pushes never publish images.

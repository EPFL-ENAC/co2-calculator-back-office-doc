# CO2 Calculator — Back Office Docs

Back-office user documentation for the EPFL ENAC CO₂ calculator, built with MkDocs and the Material theme.

- Live site: https://epfl-enac.github.io/co2-calculator-back-office-doc/
- Source docs: docs/

## Develop Locally (uv)

We recommend using [uv](https://docs.astral.sh/uv/) for fast, reproducible Python environments.

Requirements: Python 3.11+.

### 1) Install deps

```powershell
uv pip install -r requirements.txt
```

### 2) Run the docs

- Build once:

```powershell
uv run mkdocs build
```

- Serve with live reload:

```powershell
uv run mkdocs serve
```

Open http://127.0.0.1:8000/ in your browser.


## Plugins and Extensions

Configured in `mkdocs.yml`:

- Material theme features
- `pymdown-extensions` (details, superfences, math via MathJax)
- `pymdownx.snippets` for HTML/MD includes
- `table-reader` (mkdocs-table-reader-plugin) to render tabular data from files

Dependencies are pinned in [requirements.txt](requirements.txt) for consistent local and CI builds.

## CI/CD (GitHub Pages)

- Builds and deploys on pushes to `main` via [.github/workflows/deploy-docs.yml](.github/workflows/deploy-docs.yml).
- Workflow installs from [requirements.txt](requirements.txt) to ensure the same plugins are available (including `mkdocs-table-reader-plugin`).

## Contributing

- For content changes, edit files under docs/ and open a PR.
- For build/config changes, update `mkdocs.yml` and/or `requirements.txt` and verify with `uv run mkdocs build`.

## License

See [LICENSE](LICENSE).

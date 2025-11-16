# KSA Modding Wiki (Unofficial)

This repository contains the source for an **unofficial, community-driven modding wiki** for _Kitten Space Agency (KSA)_.

The site is built with [MkDocs](https://www.mkdocs.org/) and the [Material for MkDocs](https://squidfunk.github.io/mkdocs-material/) theme, and is intended to be hosted on **GitHub Pages**.

## Goals

- Provide **beginner-friendly tutorials** for creating and installing mods.
- Offer **deep technical documentation** for experienced modders and tool authors.
- Become the **main reference** for KSA modding, including internals and advanced topics where appropriate.

## Getting started (local)

1. Install Python 3 (if you don't have it already).
2. Install dependencies:

   ```bash
   pip install mkdocs mkdocs-material
   ```

3. Run the local dev server:

   ```bash
   mkdocs serve
   ```

4. Open the URL shown in the terminal (usually `http://127.0.0.1:8000/`).

## Deploying to GitHub Pages

A GitHub Actions workflow is included under `.github/workflows/deploy-docs.yml`.  
Once configured with the correct permissions, it will build and deploy the site to the `gh-pages` branch whenever you push to `main`.

## Contributing

Please see [`CONTRIBUTING.md`](CONTRIBUTING.md) for guidelines on how to propose changes or new pages.

This is a curated project: contributions are welcome via pull requests and will be reviewed before merging.

## Disclaimer

This project is **not** affiliated with or endorsed by the developers or publishers of Kitten Space Agency.

All trademarks and copyrights are the property of their respective owners.


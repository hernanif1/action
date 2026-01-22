# E2E Test and Deploy Action

A reusable GitHub Action that runs Playwright e2e tests, generates a report, deploys it to GitHub Pages, and comments on PRs.

## Usage

```yaml
name: E2E Tests

on:
  pull_request:
    branches: [ main, master ]
  push:
    branches: [ main, master ]

permissions:
  contents: write
  pages: write
  pull-requests: write

jobs:
  e2e:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Run e2e tests and deploy
        uses: hernanif1/action@main
        with:
          base_url: 'http://localhost:5173'
          test_command: 'npm run test:e2e'
          report_path: 'playwright-report'
```

## Inputs

- `base_url`: Base URL for the application (default: `http://localhost:5173`)
- `test_command`: Command to run e2e tests (default: `npm run test:e2e`)
- `report_path`: Path to the test report (default: `playwright-report`)

## Outputs

- `report_url`: URL to the published test report on GitHub Pages

## What This Action Does

1. Installs dependencies (`npm ci`)
2. Runs e2e tests using Playwright
3. Sets up GitHub Pages
4. Uploads the test report as an artifact
5. Deploys the report to GitHub Pages
6. Comments on PRs with a link to the test report

## Requirements

- The calling repository must have GitHub Pages enabled (Settings → Pages → Source: "GitHub Actions")
- The workflow must have `pages: write` and `pull-requests: write` permissions

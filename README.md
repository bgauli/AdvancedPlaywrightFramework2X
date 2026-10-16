# Advanced Playwright Framework 2X

A TypeScript end-to-end testing framework built with [Playwright](https://playwright.dev/). It is configured for environment-based base URLs, HTML and list reporting, and browser test execution in CI.

## Requirements

- Node.js 20 or later
- npm
- Playwright browser binaries (Chromium for the currently enabled project)

## Getting started

Install the project dependencies:

```bash
npm install
```

Install the Chromium browser used by the current Playwright project:

```bash
npx playwright install chromium
```

Create a `.env` file in the project root for local configuration. `.env` is ignored by Git; do not commit credentials or other secrets.

```dotenv
TTA_ENV=qa
BASE_URL=
QA_BASE_URL=https://app.thetestingacademy.com
STG_BASE_URL=https://stage.thetestingacademy.com
PROD_BASE_URL=https://app.thetestingacademy.com
DEV_BASE_URL=http://localhost:3000
API_BASE_URL=https://restful-booker.herokuapp.com
LOG_LEVEL=info
TEST_ENV=QA
TEST_AUTHOR=
```

`BASE_URL`, when non-empty, takes precedence over environment selection. Otherwise, `TTA_ENV` selects the URL:

| `TTA_ENV` value | URL variable | Fallback |
| --- | --- | --- |
| `api` | `API_BASE_URL` | `https://restful-booker.herokuapp.com` |
| `dev`, `local` | `DEV_BASE_URL` | `http://localhost:3000` |
| `stg`, `stage`, `staging` | `STG_BASE_URL` | `https://stage.thetestingacademy.com` |
| `prod`, `production` | `PROD_BASE_URL` | `https://app.thetestingacademy.com` |
| `qa` or any other value | `QA_BASE_URL` | `https://app.thetestingacademy.com` |

## Running tests

Run the complete configured test suite:

```bash
npx playwright test
```

List the tests without running them:

```bash
npx playwright test --list
```

Run only the Chromium project:

```bash
npx playwright test --project=chromium
```

Open the most recent HTML report:

```bash
npx playwright show-report
```

Tests run fully in parallel by default. The test timeout is 60 seconds and the assertion timeout is 10 seconds. On CI, tests retry twice and use one worker. Screenshots are saved on failure, video is recorded, and traces are collected on the first retry.

## Project structure

```text
.
├── .github/
│   └── workflows/       # GitHub Actions Playwright workflow
├── docs/                # Project documentation
├── rules/               # Framework or team rules
├── src/
│   ├── api/             # API clients and service helpers
│   ├── config/          # Test and environment configuration
│   ├── fixtures/        # Shared Playwright fixtures
│   ├── pages/           # Page Object Model classes
│   ├── testdata/        # Test data
│   ├── tests/           # Playwright test specifications
│   └── utils/           # Reusable utilities
├── .env                 # Local environment variables (not committed)
├── package.json         # Dependencies and npm metadata
├── playwright.config.ts # Playwright test configuration
└── tsconfig.json        # TypeScript compiler options
```

The `src` subfolders are framework organization points; add implementation files as the framework grows. The sample spec is `src/tests/example.spec.ts`.

## Configuration and browsers

Playwright configuration is in [`playwright.config.ts`](./playwright.config.ts). Chromium is the only enabled browser project at present; Firefox and WebKit are not enabled. Enable their project entries and install their browser binaries if cross-browser coverage is required.

The configuration loads `.env` with `dotenv`. CI behavior is activated by setting `CI`. The GitHub Actions workflow in `.github/workflows/playwright.yml` installs dependencies and browser binaries, runs the Playwright suite, and uploads the HTML report.

## Installed tooling

- `@playwright/test` — browser automation and test runner
- `dotenv` — load local environment variables
- `csv-parse`, `xlsx` — CSV and spreadsheet data support
- `winston` — logging
- `@faker-js/faker` — generated test data
- `jsonpath-plus` — JSONPath queries
- `ajv`, `ajv-formats` — JSON schema validation
- `allure-playwright` — Allure integration package (not currently configured as a reporter)

The active reporters are the Playwright HTML reporter and the list reporter. There are no npm test scripts yet; use the `npx playwright` commands above.

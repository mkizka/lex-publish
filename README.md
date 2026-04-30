# lex-publish

A GitHub Action to publish AT Protocol lexicons using the [goat CLI](https://github.com/bluesky-social/goat).

## Usage

### Basic (publish on push to main)

```yaml
name: Publish Lexicons
on:
  push:
    branches: [main]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
      - uses: mkizka/lex-publish@v1
        env:
          GOAT_USERNAME: ${{ secrets.GOAT_USERNAME }}
          GOAT_PASSWORD: ${{ secrets.GOAT_PASSWORD }}
```

### Lint only on pull requests

```yaml
name: Lint Lexicons
on:
  pull_request:

jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v6
      - uses: mkizka/lex-publish@v1
        with:
          publish: false
```

### Pin goat version

```yaml
- uses: mkizka/lex-publish@v1
  with:
    goat-version: "v0.2.3"
    working-directory: "./schemas"
  env:
    GOAT_USERNAME: ${{ secrets.GOAT_USERNAME }}
    GOAT_PASSWORD: ${{ secrets.GOAT_PASSWORD }}
```

## Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `goat-version` | No | `latest` | Version of goat CLI to install (e.g. `v0.2.3`) |
| `working-directory` | No | `.` | Directory containing lexicon files |
| `lint` | No | `true` | Run `goat lex lint` |
| `check-breaking` | No | `true` | Run `goat lex breaking` |
| `check-dns` | No | `true` | Run `goat lex check-dns` |
| `publish` | No | `true` | Run `goat lex publish` |

## Authentication

This action uses environment variables for goat CLI authentication. Set the following secrets in your repository settings (Settings > Secrets and variables > Actions):

| Variable | Description |
|----------|-------------|
| `GOAT_USERNAME` | Your AT Protocol handle (e.g. `user.bsky.social`) |
| `GOAT_PASSWORD` | App Password |

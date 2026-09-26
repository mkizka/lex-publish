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
      - uses: mkizka/lex-publish@v2
        with:
          paths: lexicons/com/example
          username: ${{ secrets.GOAT_USERNAME }}
          password: ${{ secrets.GOAT_PASSWORD }}
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
      - uses: mkizka/lex-publish@v2
        with:
          paths: lexicons/com/example
          publish: false
```

### Pin goat version

```yaml
- uses: mkizka/lex-publish@v2
  with:
    paths: lexicons/com/example
    goat-version: "v0.2.3"
    username: ${{ secrets.GOAT_USERNAME }}
    password: ${{ secrets.GOAT_PASSWORD }}
```

## Inputs

| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `goat-version` | No | `latest` | Version of goat CLI to install (e.g. `v0.2.3`) |
| `working-directory` | No | `.` | Directory containing lexicon files |
| `paths` | Yes | | Lexicon file or directory you own (e.g. `lexicons/com/example`) |
| `lint` | No | `true` | Run `goat lex lint` |
| `check-breaking` | No | `true` | Run `goat lex breaking` |
| `check-dns` | No | `true` | Run `goat lex check-dns` |
| `publish` | No | `true` | Run `goat lex publish` |
| `update` | No | `true` | Pass `--update` flag to `goat lex publish` to update existing lexicons |
| `username` | When `publish` is `true` | | Your AT Protocol handle (e.g. `user.bsky.social`) |
| `password` | When `publish` is `true` | | App Password. Store it in repository secrets |

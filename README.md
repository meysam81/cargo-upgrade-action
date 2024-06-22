# Cargo Upgrades Action

This GitHub Action allows you to have continuous
integration for the newer version of your `Cargo.toml`
dependencies.

## Usage

```yaml
name: ci

on:
  schedule:
    - cron: "30 1 * * *"

jobs:
  cargo-upgrade:
    if: github.event_name == 'schedule'
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - name: Checkout code
        uses: actions/checkout@v4
      - name: Cache
        uses: actions/cache@v4
        with:
          path: |
            ~/.cargo/registry
            ~/.cargo/git
            target
          key: ${{ runner.os }}-cargo-${{ hashFiles('**/Cargo.lock') }}
      - name: Run cargo-upgrade
        uses: meysam81/cargo-upgrade-action@v1
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
```

## Inputs

| Name                    | Description                                                                                            | Default |
| ----------------------- | ------------------------------------------------------------------------------------------------------ | ------- |
| `cargo-upgrade-version` | The version of `cargo-upgrade` to use.                                                                 | `~0.12` |
| `extra-flags`           | Extra flags to pass to `cargo-upgrade`. For example `-i allow`                                         |         |
| `token`                 | GitHub token. Required for creating the pull request. the `pull-requests: write` permission is needed. |         |

# App auth GitHub action

GitHub action for GitHub App authentication. It generates an auth token from app's private key to be used in some another workflow's step.

## Usage

Put `topmonks/app-auth-action@<VERSION>` into `uses` key of the step.

Action has two inputs:
- `appId`: ID of the GitHub app
- `appPrivateKey`: Private key of the GitHub app

The generated token will be in steps output `token`.

Example:

```yaml
name: Add issues to project

on:
  issues:
    types:
      - opened

jobs:
  add-to-project:
    name: Add issue to project
    runs-on: ubuntu-latest
    steps:
      - name: Get token
        id: get-token
        uses: topmonks/app-auth-action@v1
        with:
          appId: ${{ secrets.APP_ID }}
          appPrivateKey: ${{ secrets.APP_PRIVATE_KEY }}
      - name: Add issue to project
        uses: actions/add-to-project@v0.4.0
        with:
          project-url: <PROJECT-URL>
          github-token: ${{ steps.get-token.outputs.token }}
```

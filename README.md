# usc-rotate-keys-gha

A Github action for rotating the keys for `usc`.

## Setup

- Add an action below to your workflow.
- Add the required secrets, the AWS keys, to your Github project.
- If your names are different from the defaults change them.
- Push it, then watch the Action tab on Github.

### Action

```yaml
# Rotate keys in current repo with default names
- name: Rotate keys default names
  uses: mammutmw/usc-rotate-keys-gha@v1.0.0
  with:
    aws_access_key: ${{secrets.AWS_ACCESS_KEY_ID}}
    aws_secret_access_key: ${{secrets.AWS_SECRET_ACCESS_KEY}}


# Rotate keys in current repo with changed names
- name: Rotate keys default names
  uses: mammutmw/usc-rotate-keys-gha@v1.0.0
  with:
    aws_access_key: ${{secrets.USC_KEY}}
    aws_secret_access_key: ${{secrets.USC_SECRET}}
    key: 'USC_KEY'
    secret: 'USC_SECRET'
```

### Example workflow

Here's a full example.

```yaml
# .github/workflows/rotate-keys.yml
name: Rotate keys
on:
  schedule: # Run every other month
    - cron: '0 11 1 */2 *'
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Dump GitHub context, for debugging
        env:
          GITHUB_CONTEXT: ${{ toJson(github) }}
        run: echo "$GITHUB_CONTEXT"

        # Rotate keys
      - name: Rotate Keys
        uses: mammutmw/usc-rotate-keys-gha@v1.0.0
        with:
          aws_access_key: ${{secrets.AWS_ACCESS_KEY_ID}}
          aws_secret_access_key: ${{secrets.AWS_SECRET_ACCESS_KEY}}
```

### Parameters

| Name | Description | Default |
-------|-------------|----------|
| aws_access_key | The AWS_ACCESS_KEY_ID | required |
| aws_secret_access_key | The AWS_SECRET_ACCESS_KEY | required |
| key | The name of the key to update | AWS_ACCESS_KEY_ID |
| secret | The name of the secret to update | AWS_SECRET_ACCESS_KEY |
| project | The name of the repo with the secrets | current repo |
| token | The github token to use | current GITHUB_TOKEN |

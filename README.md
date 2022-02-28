# usc-rotate-keys-gha

A Github action for rotating the keys for `usc`.

## Example workflow

Here's a full example.

```yaml
# .github/workflows/rotate-keys.yml
name: Rotate keys
on:
  schedule: # Run at 11:22 on the first every other month (the token is old after 90 days)
    - cron: "22 11 1 */2 *"
  workflow_dispatch: # Allow running from the UI

jobs:
  rotate:
    runs-on: ubuntu-latest
    steps:
      - name: Rotate Keys
        uses: ingka-group-digital/usc-rotate-keys-gha@latest
        with:
          aws_access_key: ${{secrets.AWS_ACCESS_KEY_ID}}
          aws_secret_access_key: ${{secrets.AWS_SECRET_ACCESS_KEY}}
          token: ${{secrets.PAT}} # PAT with full repo access and SSO
```

## Setup

- Add a workflow for rotating keys (example above).
- Create a [Github Personal Access Token (PAT)](https://github.com/settings/tokens) with full repo access.
- [Authorize the PAT for use with SAML single sign-on](https://docs.github.com/en/github/authenticating-to-github/authenticating-with-saml-single-sign-on/authorizing-a-personal-access-token-for-use-with-saml-single-sign-on).
- Add the required secrets (:gear:**Settings|Secrets**):
  - `AWS_ACCESS_KEY_ID`
  - `AWS_SECRET_ACCESS_KEY`
  - `PAT` (the above PAT)
- If your names are different from the defaults, change them.
- Push it, then test the flow by manually triggering it.

### Example Action Configurations

Default configuration only rotates the keys for current repo. If using
the keys in more than one project, you must explicitly give a comma separated
list of repos as `project:` arguments (third example below).

```yaml
# Rotate keys in current repo with default names
- name: Rotate keys default names
  uses: ingka-group-digital/usc-rotate-keys-gha@latest
  with:
    aws_access_key: ${{secrets.AWS_ACCESS_KEY_ID}}
    aws_secret_access_key: ${{secrets.AWS_SECRET_ACCESS_KEY}}
    token: ${{secrets.PAT}} # PAT with full repo access and SSO

# Rotate keys in current repo with changed names
- name: Rotate keys default names
  uses: ingka-group-digital/usc-rotate-keys-gha@latest
  with:
    aws_access_key: ${{secrets.USC_KEY}}
    aws_secret_access_key: ${{secrets.USC_SECRET}}
    key: "USC_KEY"
    secret: "USC_SECRET"
    token: ${{secrets.PAT}} # PAT with full repo access and SSO

# Rotate keys in multiple repos with default names
- name: Rotate keys default names
  uses: ingka-group-digital/usc-rotate-keys-gha@latest
  with:
    aws_access_key: ${{secrets.AWS_ACCESS_KEY_ID}}
    aws_secret_access_key: ${{secrets.AWS_SECRET_ACCESS_KEY}}
    token: ${{secrets.PAT}} # PAT with full repo access and SSO
    project: "owner/repo1,owner/repo2,owner/repo3"

# Rotate keys in organization
- name: Rotate keys default names
  uses: ingka-group-digital/usc-rotate-keys-gha@latest
  with:
    aws_access_key: ${{secrets.AWS_ACCESS_KEY_ID}}
    aws_secret_access_key: ${{secrets.AWS_SECRET_ACCESS_KEY}}
    token: ${{secrets.PAT}} # PAT with full repo access and SSO
    provider: "githuborg"
    project: "org"
```

### Parameters

| Name                  | Description                                                                       | Default               |
| --------------------- | --------------------------------------------------------------------------------- | --------------------- |
| aws_access_key        | The AWS_ACCESS_KEY_ID                                                             | required              |
| aws_secret_access_key | The AWS_SECRET_ACCESS_KEY                                                         | required              |
| key                   | The name of the key to update                                                     | AWS_ACCESS_KEY_ID     |
| secret                | The name of the secret to update                                                  | AWS_SECRET_ACCESS_KEY |
| project               | The name of the repo with the secrets or a comma separated list of multiple repos | current repo          |
| token                 | A Github Personal Access Token (PAT)                                              | required              |
| provider              | Type of secre to update: github, githuborg or cloudbuild                          | github                |

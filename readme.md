# 🤖 defaults

A GitHub action that sets git defaults (name, email, author) for 
automated bot workflows from secrets or environment variables.

Also defaults `GH_TOKEN` to `GITHUB_TOKEN` if empty. 

## Usage

```
- name: 🤖 defaults
  uses: devlooped/actions-bot@v1
  with:
    # The default name of the bot account. 
    # Defaults to $GITHUB_ACTOR.
    # Set as $BOT_NAME environment variable after run, and as bot-name output.
    name: ''

    # The default email of the bot account. 
    # Defaults to $GITHUB_ACTOR@users.noreply.github.com.
    # Set as $BOT_EMAIL environment variable after run, and as bot-email output.
    email: ''

    # The token to set as $GH_TOKEN environment variable. If empty, 
    # uses the github_token input as a fallback.
    # Defaults to ''. Typically set to ${{ secrets.GH_TOKEN }}.
    # Set as $GH_TOKEN and BOT_TOKEN environment variables after run, 
    # and as bot-token output.
    gh_token: ''

    # Fallback token to use for $GH_TOKEN if it's empty.
    # Defaults to ''. Typically set to ${{ secrets.GITHUB_TOKEN }}.
    github_token: ''
```

If both `gh_token` and `github_token` have empty values, the action will fail.

The additional `BOT_AUTHOR` environment variable is set to `"$BOT_NAME <$BOT_EMAIL>"` 
as well as the `bot-author` output for easy consumption.

The action also runs:

```
git config --global user.name $BOT_NAME
git config --global user.email $BOT_EMAIL
```

so that git commands can be run the resulting defaults already applied.

## Example

To run the action and automatically create a PR with the resolved bot defaults:

```yml
on: 
  push:
    branches:
      - main

jobs:
  includes:
    runs-on: ubuntu-latest
    steps:
      - name: 🤖 defaults
        uses: devlooped/actions-bot@v1
        with:
          name: ${{ secrets.BOT_NAME }}
          email: ${{ secrets.BOT_EMAIL }}
          gh_token: ${{ secrets.GH_TOKEN }}
          github_token: ${{ secrets.GITHUB_TOKEN }}

      - name: 🤘 checkout
        uses: actions/checkout@v2
        with:
          token: ${{ env.GH_TOKEN }}

      # add some step that changes files

      - name: ✍ pull request
        uses: peter-evans/create-pull-request@v3
        with:
          base: main
          branch: bot-updates
          author: ${{ env.BOT_AUTHOR }}
          committer: ${{ env.BOT_AUTHOR }}
          commit-message: ⬆️ Bot file updates
          title: ⬆️ Bot file updates
          body: Updates made by @${{ env.BOT_NAME }}.
          token: ${{ env.GH_TOKEN }}
```

In a repository that doesn't define any of the custom secrets passed to 
the `devlooped/actions-bot@v1` action, all environment variables will be 
properly defaulted, since at least `${{ secrets.GITHUB_TOKEN }}` *will* 
be present, as well as the `$GITHUB_ACTOR` which is used to default 
`BOT_NAME`, `BOT_EMAIL` and `BOT_AUTHOR`.

It would also be possible to just run `git` commands from a shell and 
get the same defaults automatically.

<!-- include https://github.com/devlooped/sponsors/raw/main/footer.md -->
# Sponsors 

<!-- sponsors.md -->
[![Clarius Org](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/clarius.png "Clarius Org")](https://github.com/clarius)
[![Christian Findlay](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/MelbourneDeveloper.png "Christian Findlay")](https://github.com/MelbourneDeveloper)
[![C. Augusto Proiete](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/augustoproiete.png "C. Augusto Proiete")](https://github.com/augustoproiete)
[![Kirill Osenkov](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/KirillOsenkov.png "Kirill Osenkov")](https://github.com/KirillOsenkov)
[![MFB Technologies, Inc.](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/MFB-Technologies-Inc.png "MFB Technologies, Inc.")](https://github.com/MFB-Technologies-Inc)
[![SandRock](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/sandrock.png "SandRock")](https://github.com/sandrock)
[![Andy Gocke](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/agocke.png "Andy Gocke")](https://github.com/agocke)
[![Shahzad Huq](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/shahzadhuq.png "Shahzad Huq")](https://github.com/shahzadhuq)


<!-- sponsors.md -->

[![Sponsor this project](https://raw.githubusercontent.com/devlooped/sponsors/main/sponsor.png "Sponsor this project")](https://github.com/sponsors/devlooped)
&nbsp;

[Learn more about GitHub Sponsors](https://github.com/sponsors)

<!-- https://github.com/devlooped/sponsors/raw/main/footer.md -->

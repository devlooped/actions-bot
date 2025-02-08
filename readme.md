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

so that git commands can be run with the resulting defaults already applied.

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
[![MFB Technologies, Inc.](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/MFB-Technologies-Inc.png "MFB Technologies, Inc.")](https://github.com/MFB-Technologies-Inc)
[![Torutek](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/torutek-gh.png "Torutek")](https://github.com/torutek-gh)
[![DRIVE.NET, Inc.](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/drivenet.png "DRIVE.NET, Inc.")](https://github.com/drivenet)
[![Keith Pickford](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/Keflon.png "Keith Pickford")](https://github.com/Keflon)
[![Thomas Bolon](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/tbolon.png "Thomas Bolon")](https://github.com/tbolon)
[![Kori Francis](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/kfrancis.png "Kori Francis")](https://github.com/kfrancis)
[![Toni Wenzel](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/twenzel.png "Toni Wenzel")](https://github.com/twenzel)
[![Uno Platform](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/unoplatform.png "Uno Platform")](https://github.com/unoplatform)
[![Dan Siegel](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/dansiegel.png "Dan Siegel")](https://github.com/dansiegel)
[![Reuben Swartz](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/rbnswartz.png "Reuben Swartz")](https://github.com/rbnswartz)
[![Jacob Foshee](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/jfoshee.png "Jacob Foshee")](https://github.com/jfoshee)
[![](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/Mrxx99.png "")](https://github.com/Mrxx99)
[![Eric Johnson](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/eajhnsn1.png "Eric Johnson")](https://github.com/eajhnsn1)
[![Ix Technologies B.V.](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/IxTechnologies.png "Ix Technologies B.V.")](https://github.com/IxTechnologies)
[![David JENNI](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/davidjenni.png "David JENNI")](https://github.com/davidjenni)
[![Jonathan ](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/Jonathan-Hickey.png "Jonathan ")](https://github.com/Jonathan-Hickey)
[![Charley Wu](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/akunzai.png "Charley Wu")](https://github.com/akunzai)
[![Jakob Tikjøb Andersen](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/jakobt.png "Jakob Tikjøb Andersen")](https://github.com/jakobt)
[![Tino Hager](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/tinohager.png "Tino Hager")](https://github.com/tinohager)
[![Ken Bonny](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/KenBonny.png "Ken Bonny")](https://github.com/KenBonny)
[![Simon Cropp](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/SimonCropp.png "Simon Cropp")](https://github.com/SimonCropp)
[![agileworks-eu](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/agileworks-eu.png "agileworks-eu")](https://github.com/agileworks-eu)
[![sorahex](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/sorahex.png "sorahex")](https://github.com/sorahex)
[![Zheyu Shen](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/arsdragonfly.png "Zheyu Shen")](https://github.com/arsdragonfly)
[![Vezel](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/vezel-dev.png "Vezel")](https://github.com/vezel-dev)
[![ChilliCream](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/ChilliCream.png "ChilliCream")](https://github.com/ChilliCream)
[![4OTC](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/4OTC.png "4OTC")](https://github.com/4OTC)
[![Vincent Limo](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/v-limo.png "Vincent Limo")](https://github.com/v-limo)
[![Jordan S. Jones](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/jordansjones.png "Jordan S. Jones")](https://github.com/jordansjones)
[![domischell](https://raw.githubusercontent.com/devlooped/sponsors/main/.github/avatars/DominicSchell.png "domischell")](https://github.com/DominicSchell)


<!-- sponsors.md -->

[![Sponsor this project](https://raw.githubusercontent.com/devlooped/sponsors/main/sponsor.png "Sponsor this project")](https://github.com/sponsors/devlooped)
&nbsp;

[Learn more about GitHub Sponsors](https://github.com/sponsors)

<!-- https://github.com/devlooped/sponsors/raw/main/footer.md -->

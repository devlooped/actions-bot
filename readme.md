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
[![Clarius Org](https://avatars.githubusercontent.com/u/71888636?v=4&s=39 "Clarius Org")](https://github.com/clarius)
[![MFB Technologies, Inc.](https://avatars.githubusercontent.com/u/87181630?v=4&s=39 "MFB Technologies, Inc.")](https://github.com/MFB-Technologies-Inc)
[![SandRock](https://avatars.githubusercontent.com/u/321868?u=99e50a714276c43ae820632f1da88cb71632ec97&v=4&s=39 "SandRock")](https://github.com/sandrock)
[![DRIVE.NET, Inc.](https://avatars.githubusercontent.com/u/15047123?v=4&s=39 "DRIVE.NET, Inc.")](https://github.com/drivenet)
[![Keith Pickford](https://avatars.githubusercontent.com/u/16598898?u=64416b80caf7092a885f60bb31612270bffc9598&v=4&s=39 "Keith Pickford")](https://github.com/Keflon)
[![Thomas Bolon](https://avatars.githubusercontent.com/u/127185?u=7f50babfc888675e37feb80851a4e9708f573386&v=4&s=39 "Thomas Bolon")](https://github.com/tbolon)
[![Kori Francis](https://avatars.githubusercontent.com/u/67574?u=3991fb983e1c399edf39aebc00a9f9cd425703bd&v=4&s=39 "Kori Francis")](https://github.com/kfrancis)
[![Reuben Swartz](https://avatars.githubusercontent.com/u/724704?u=2076fe336f9f6ad678009f1595cbea434b0c5a41&v=4&s=39 "Reuben Swartz")](https://github.com/rbnswartz)
[![Jacob Foshee](https://avatars.githubusercontent.com/u/480334?v=4&s=39 "Jacob Foshee")](https://github.com/jfoshee)
[![](https://avatars.githubusercontent.com/u/33566379?u=bf62e2b46435a267fa246a64537870fd2449410f&v=4&s=39 "")](https://github.com/Mrxx99)
[![Eric Johnson](https://avatars.githubusercontent.com/u/26369281?u=41b560c2bc493149b32d384b960e0948c78767ab&v=4&s=39 "Eric Johnson")](https://github.com/eajhnsn1)
[![Jonathan ](https://avatars.githubusercontent.com/u/5510103?u=98dcfbef3f32de629d30f1f418a095bf09e14891&v=4&s=39 "Jonathan ")](https://github.com/Jonathan-Hickey)
[![Ken Bonny](https://avatars.githubusercontent.com/u/6417376?u=569af445b6f387917029ffb5129e9cf9f6f68421&v=4&s=39 "Ken Bonny")](https://github.com/KenBonny)
[![Simon Cropp](https://avatars.githubusercontent.com/u/122666?v=4&s=39 "Simon Cropp")](https://github.com/SimonCropp)
[![agileworks-eu](https://avatars.githubusercontent.com/u/5989304?v=4&s=39 "agileworks-eu")](https://github.com/agileworks-eu)
[![Zheyu Shen](https://avatars.githubusercontent.com/u/4067473?v=4&s=39 "Zheyu Shen")](https://github.com/arsdragonfly)
[![Vezel](https://avatars.githubusercontent.com/u/87844133?v=4&s=39 "Vezel")](https://github.com/vezel-dev)
[![ChilliCream](https://avatars.githubusercontent.com/u/16239022?v=4&s=39 "ChilliCream")](https://github.com/ChilliCream)
[![4OTC](https://avatars.githubusercontent.com/u/68428092?v=4&s=39 "4OTC")](https://github.com/4OTC)
[![domischell](https://avatars.githubusercontent.com/u/66068846?u=0a5c5e2e7d90f15ea657bc660f175605935c5bea&v=4&s=39 "domischell")](https://github.com/DominicSchell)
[![Adrian Alonso](https://avatars.githubusercontent.com/u/2027083?u=129cf516d99f5cb2fd0f4a0787a069f3446b7522&v=4&s=39 "Adrian Alonso")](https://github.com/adalon)
[![torutek](https://avatars.githubusercontent.com/u/33917059?v=4&s=39 "torutek")](https://github.com/torutek)
[![Ryan McCaffery](https://avatars.githubusercontent.com/u/16667079?u=c0daa64bb5c1b572130e05ae2b6f609ecc912d4d&v=4&s=39 "Ryan McCaffery")](https://github.com/mccaffers)
[![Seika Logiciel](https://avatars.githubusercontent.com/u/2564602?v=4&s=39 "Seika Logiciel")](https://github.com/SeikaLogiciel)
[![Andrew Grant](https://avatars.githubusercontent.com/devlooped-user?s=39 "Andrew Grant")](https://github.com/wizardness)
[![eska-gmbh](https://avatars.githubusercontent.com/devlooped-team?s=39 "eska-gmbh")](https://github.com/eska-gmbh)
[![Geodata AS](https://avatars.githubusercontent.com/u/5946299?v=4&s=39 "Geodata AS")](https://github.com/geodata-no)


<!-- sponsors.md -->
[![Sponsor this project](https://avatars.githubusercontent.com/devlooped-sponsor?s=118 "Sponsor this project")](https://github.com/sponsors/devlooped)

[Learn more about GitHub Sponsors](https://github.com/sponsors)

<!-- https://github.com/devlooped/sponsors/raw/main/footer.md -->

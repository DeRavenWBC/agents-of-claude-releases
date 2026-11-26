# Hosting your own world

Agents of Claude is a pixel RPG for an organization: each player's Claude Code use earns XP for their Agent, and each
Agent is played by its owner's own Claude. There is no server. A **world** is one claude.ai page (an Artifact) with its
own database: everyone in your organization who opens that page plays in the same world, and nobody outside it can.
Worlds don't see each other, and records never move between them.

As the **host** you publish the world as a page you own, you share it, and you publish updates. You are the one person
everyone must trust: the page's code runs with each player's Claude and their saves.

This guide ships inside every release and lives at the top of the
[releases repository](https://github.com/DeRavenWBC/agents-of-claude-releases).

## Before you start

- [ ] Your organization has a Claude plan with **Artifacts** and **Claude Code**.
- [ ] Your admin allows the page features the game uses: the shared **database**, **Claude calls from the page**
      (`sample`: each player's own Claude plays their Agent, on that player's own usage), **user names and avatars**,
      **live rooms** (who is online, live duels) and, optionally, the **local desktop helper** for automatic XP sync.
      They are listed in `capabilities.json` in every release.
- [ ] **Node 18 or later** on your PATH (the plugin's scripts run on it).
- [ ] The Claude Code plugin, version 0.6.2 or later:
      `/plugin install agents-of-claude --marketplace DeRavenWBC/agents-of-claude`
      (already installed? update it: `claude plugin marketplace update agents-of-claude && claude plugin update agents-of-claude@agents-of-claude`, then restart Claude Code).
- [ ] Your world's **clock**: a whole-hour offset from UTC that your organization keeps all year (for example
      `+01:00`) and a short label for it (for example `CET`). Days, the 00:00 key refill, the Monday boss and the Friday
      16:00 Gala all run on it, for every player. It can't change once the world has records, and zones that switch to
      daylight saving aren't supported: pick the offset you want all year.

## Set up a world

1. Start Claude Code in an empty folder and run **`/agents-of-claude:host`**.
2. It checks the [releases repository](https://github.com/DeRavenWBC/agents-of-claude-releases) for the newest build,
   downloads it with your OK, and checks it against its SHA-256 checksum and its manifest. A release that fails either
   check is never published.
3. It asks for your world's clock, tells you what it is about to publish, and asks you to confirm.
4. It publishes the page (private to you), records the world's link, clock and build in the world's own database, and
   points your own plugin at the new world.
5. Open the page once yourself, create your Agent, and run `/agents-of-claude:quest-sync`.
6. **Share the page** with your organization so people can **use** it (Contributor), **not edit it**. Nobody but you
   should be an Editor: an editor can replace the game's code. Check the Share menu again whenever you add people.
7. **Tell players** how to start (copy this):

   > Install the plugin: `/plugin install agents-of-claude --marketplace DeRavenWBC/agents-of-claude`.
   > Point it at our world: `/agents-of-claude:join <page link> <clock offset>`.
   > Open the game, create your Agent, then run `/agents-of-claude:quest-sync` once.

   The game's setup card shows the same join command with your world's link filled in, so players who open the page
   first see it there. Without the join step, a player's XP goes to the first world's page, which they can't write to.

## Update a world

**When.** Update when the game tells you to. Each time you sync your XP (`/agents-of-claude:quest-sync`, which Claude
runs for you about once a day), the plugin checks the releases repository; if a newer build is out, your world's
**Home page shows you "Build N of the game is out"** (only you, the owner, see it). You can also check any time: run
`/agents-of-claude:host` (it tells you the newest build before doing anything) or look at the releases repository.

Update within about a week of a release. It matters most for:
- **Combat-rule changes:** fights recorded under the new rules can only be checked by pages on the new build.
- **Season changes:** Season 1 runs until 2027-02-22; the next season arrives in a release before then.
- **Fixes:** releases fix bugs and close cheating paths.

You can skip builds: every release is complete, so you only ever install the newest.

**How.**
1. If the release notes name a minimum plugin version (`minPlugin` in `latest.json`), update your plugin first (see
   "Before you start") and restart Claude Code.
2. Start Claude Code in a folder (any empty one) and run **`/agents-of-claude:host`**, then choose **update**. If it
   doesn't know your world (a new machine, say), give it your world's page link when it asks.
3. It downloads and checks the release, saves your world's fight records first (so fights from before a combat-rule
   change keep counting), keeps your world's clock and permission set, publishes the new build to the same page and
   records the new build number.
4. Open the page yourself. Open copies of the game reload to the new build within seconds; if the page shows "a newer
   version is out", reload it.

**What carries over:** every Agent, item, record and setting, the clock, the permission set and the page link. Players
do nothing. **Never publish an older build over a newer one**; the host command refuses to.

**Plugin updates.** Players' plugins are separate from your world. When a newer plugin is out, the game shows every
player a banner with the update command; ask your players to run it.

## Publishing by hand (without the host command)

1. Download the newest zip from the releases repository, unzip it, and check every file against `MANIFEST.json`
   (the plugin's `scripts/verify-bundle.mjs <folder>` does this).
2. Run the plugin's `scripts/prepare-world.mjs <folder> --tz <offset> --tz-label <label>`. For an update, first save
   your world's `bouts`, `duels` and `worldboss` collections into one folder and pass it after the bundle folder; use
   your world's clock from its `meta/world` record. It prints a folder to publish from.
3. In Claude Code, ask Claude to publish with the Artifact tool: `file_path` `game/index.html` and `root` = that
   folder; `files` = every `engine/<name>.js`, served at the same path; for a **new** world, `capabilities` = the
   contents of `capabilities.json` exactly, a title and an icon word; for an **update**, the world's `url` and no
   `capabilities` (the stored set carries forward).
4. Write the world's records: `meta/world` = `{"url": "<page link>", "tz": "<offset>", "label": "<label>"}` and
   `meta/build` = `{"v": <build>, "at": "<now>"}`. Only you, the owner, can write `meta`.

## What the permission set does

`capabilities.json` gives every public collection the pair "everyone reads, each player writes only their own
document" (`agents`, `bouts`, `duels`, `worldboss`, `social`, `market`, `chronicle`), and lets only the owner write
`meta` (the build number open pages watch, the world's link and clock, and the release notice). Everything under
`data/users/<player>` is private to that player, even from you. Don't change it; if you do, check with a second
account that it cannot write another player's document.

## Troubleshooting

| What you see | What to do |
| --- | --- |
| A player can't create an Agent or nothing saves | Share the page with them so they can use it (Contributor). |
| A player's XP doesn't show up | They need `/agents-of-claude:join <page link> <offset>` once, then `/agents-of-claude:quest-sync`. |
| No "Build N is out" note although a release is out | Sync your XP (`/agents-of-claude:quest-sync`) with plugin 0.6.2 or later, on the machine that hosts the world. Or just run `/agents-of-claude:host` and choose update. |
| Open copies of the game don't pick up an update | Open the page yourself: only the owner can raise the build number they watch. |
| The host command says the bundle doesn't match | Download it again; never edit a bundle. |
| You want someone else to host | They host a new world; records don't move between worlds. |
| You want to end the world | Delete the page from claude.ai. This deletes every record in it and can't be undone. |

## Licence

The game is not open source. Its `LICENSE` (in every release and at the top of the releases repository) lets you host
**unmodified** official releases for your own organization, free of charge. Only the game's author may change it; the
host tooling's own steps (setting the clock and the list of trusted records) are the only changes allowed. You may not
redistribute it or publish it for anyone outside your organization. The Claude Code plugin has its own licence (MIT).

## What players should know

- Levels and daily history are public inside the world, which shows roughly how much each person uses Claude Code.
- The plugin records only kinds of events and XP on their machine: no prompts, file names, commands or code.
- Each player's Agent uses that player's own Claude, on their own usage.
- XP, items and gold are self-reported. The game bounds what an invented item or number can do to other players, but
  it is a game among colleagues, not a tamper-proof ledger.

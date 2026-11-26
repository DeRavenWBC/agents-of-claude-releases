# Agents of Claude: world releases

Agents of Claude is a pixel RPG for an organization: everyone's Claude Code use earns XP for their Agent, and each
Agent is played by its owner's own Claude. There is no server: a world is one claude.ai page with its own database, and
each organization hosts its own.

This repository holds only the releases a host publishes. Each zip is a complete world bundle: the page, its engine,
the permission set it needs, `MANIFEST.json` with a SHA-256 of every file, `HOSTING.md` and `LICENSE`.
`latest.json` names the newest build, its zip, the zip's checksum and the minimum plugin version it needs.

**Read [HOSTING.md](HOSTING.md) first:** it is the complete guide to setting up, sharing, updating and running a world.

## Set up a world (short version)

1. Install the Claude Code plugin (Node 18 or later):
   `/plugin install agents-of-claude --marketplace DeRavenWBC/agents-of-claude`
2. Decide your world's clock: a whole-hour offset from UTC your organization keeps all year (for example `+01:00 CET`).
3. Start Claude Code in an empty folder and run `/agents-of-claude:host`. It downloads the newest release, checks it,
   asks for the clock, publishes the page as yours and tells you what to share.
4. Share the page with your organization so people can **use** it, not edit it, and give players the join command it
   prints (the game's setup card shows it too).

## When and how to update

- **When:** your world's Home page tells you "Build N of the game is out". The plugin checks this repository whenever
  you sync your XP (Claude does that about once a day). Update within about a week, and always before a season ends
  (Season 1 ends 2027-02-22). You can skip builds: each release is complete.
- **How:** run `/agents-of-claude:host` and choose **update**. If `latest.json` names a `minPlugin`, update the plugin
  first. Your world's records, clock, permission set and link carry over; players do nothing.

## Licence

All rights reserved. Organizations may host **unmodified** releases for their own members, free of charge; only the
author may change the game. See [LICENSE](LICENSE). The Claude Code plugin has a matching licence: install and use it
unmodified from its official marketplace; only the author may change it.

## Releases

<!-- releases -->

- [agents-of-claude-world-b148.zip](releases/agents-of-claude-world-b148.zip) (latest: The Stable is its own tab under Bag.)
- [agents-of-claude-world-b147.zip](releases/agents-of-claude-world-b147.zip)
- [agents-of-claude-world-b146.zip](releases/agents-of-claude-world-b146.zip)
- [agents-of-claude-world-b145.zip](releases/agents-of-claude-world-b145.zip)
- [agents-of-claude-world-b144.zip](releases/agents-of-claude-world-b144.zip)
- [agents-of-claude-world-b143.zip](releases/agents-of-claude-world-b143.zip)
- [agents-of-claude-world-b142.zip](releases/agents-of-claude-world-b142.zip)
- [agents-of-claude-world-b141.zip](releases/agents-of-claude-world-b141.zip)

# Agents of Claude: world releases

Agents of Claude is a pixel RPG for an organization: everyone's Claude Code use earns XP for their Agent, and each
Agent is played by its owner's own Claude. There is no server: a world is one claude.ai page with its own database, and
each organization hosts its own.

This repository holds only the releases a host publishes. Each zip is a complete world bundle (the page, its engine,
the permission set it needs, `MANIFEST.json` with a SHA-256 of every file, and `HOSTING.md`). `latest.json` names the
newest one and its checksum.

## Host a world

1. Install the Claude Code plugin: `/plugin install agents-of-claude --marketplace DeRavenWBC/agents-of-claude`.
2. Start Claude Code in an empty folder and run `/agents-of-claude:host`. It downloads the newest release, checks it
   against its checksum and manifest, asks for your world's clock, publishes the page as yours, and tells you what to
   share. Read `HOSTING.md` in the bundle for what you are responsible for as a host.

## Update a world

The game tells its owner on the Home page when a newer build is out (it checks `latest.json` here once a day). Run
`/agents-of-claude:host` again and choose "update": your world's records, clock and permission set carry over.

## Releases

<!-- releases -->

- [agents-of-claude-world-b142.zip](releases/agents-of-claude-world-b142.zip) (latest: No gameplay changes.)
- [agents-of-claude-world-b141.zip](releases/agents-of-claude-world-b141.zip)

# Chess — Setup guide

This guide explains how to install and configure Chess in Bot Designer For Discord (BDFD) using Components V2.

## Prerequisites

- A bot created in BDFD.
- Components V2 enabled for your bot.
- The variable `chess_state` created (default value: empty).

## Variables to create

In your BDFD dashboard, go to **Variables** and create:

| Name | Value |
|------|-------|
| `chess_state` | (leave empty) |

This variable stores the full state of each game: `fen|whiteID|blackID|gameID|status|lastMove|lastMoveSAN|theme`.

## External API

This game requires a stateless HTTP API to validate moves and render the board. The API used is `http://chess.nexusify.co`.

## Installation

### 1. Create the variable

Create `chess_state` with an empty default value.

### 2. Create the three commands

You need to create 3 commands in BDFD:

1. **Main command** — see [`Chess_Command.md`](./Chess_Command.md)
2. **Callback 1** — see [`Callback 1 ($onInteraction).md`](./Callback%201%20%28$onInteraction%29.md)
3. **Callback 2** — see [`Callback 2 ($onInteraction).md`](./Callback%202%20%28$onInteraction%29.md)

For each command, copy the trigger and the code from the corresponding file.

### 3. Play!

Type `!chess @opponent` in any channel where your bot can read messages.

## Notes

- Game state is stored per user (challenger), so multiple users can play simultaneously.
- Each game has a unique game ID. If you start a new game, the old game's buttons stop working.
- The challenger can choose the board theme (Green, Blue, Brown, Purple) before the opponent accepts.
- Use the 🏳️ Resign button to forfeit, or 🤝 Offer draw to propose a tie.

## Slash Command (optional)

If you want to use `/chess` in addition to `!chess`, configure the slash command in BDFD:

| Field | Value |
|-------|-------|
| Option name | `user` |
| Option type | User |
| Option Required | Yes |

Once configured, users can use `/chess user:@opponent`.

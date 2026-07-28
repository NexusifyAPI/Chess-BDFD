# Chess — BDFD

A 1v1 Chess game for Discord bots built with **Bot Designer For Discord (BDFD)** using **Components V2**. Features board theme selector (Green/Blue/Brown/Purple), flag mode, draw offers, resignation, automatic check/checkmate/stalemate detection, per-user game state, unique game IDs, and 6 language localizations (Spanish, English, German, Italian, Portuguese, French).

This game requires an external stateless chess API (`http://chess.nexusify.co`) for move validation and board rendering.

## Variables to create in BDFD

Before installing the commands, create this variable in your BDFD dashboard (**Variables** section):

| Name | Value |
|------|-------|
| `chess_state` | (leave empty) |

The variable stores the full game state per user in the format `fen|whiteID|blackID|gameID|status|lastMove|lastMoveSAN|theme`.

## Available languages

Each language folder contains a localized setup guide and the three command files (main command + two interaction callbacks).

| Language | Folder | Setup guide |
|----------|--------|-------------|
| Español  | `Español/`   | [LEEME.md](./Español/LEEME.md) |
| English  | `English/`   | [README.md](./English/README.md) |
| Deutsch  | `Deutsch/`   | [LIESMICH.md](./Deutsch/LIESMICH.md) |
| Italiano | `Italiano/`  | [LEGGIMI.md](./Italiano/LEGGIMI.md) |
| Português | `Português/` | [LEIAME.md](./Português/LEIAME.md) |
| Français | `Français/`  | [LISEZMOI.md](./Français/LISEZMOI.md) |

## Features

- **1v1 Chess** with full rules: castling, en passant, promotion, check/checkmate/stalemate detection
- **Board theme selector**: challenger picks from Green, Blue, Brown, or Purple before the opponent accepts
- **Resign**: either player can forfeit at any time
- **Draw offers**: active player offers draw, opponent accepts or declines
- **Per-user state**: stored in the `chess_state` variable
- **Game ID system**: each game gets a unique ID; stale button clicks from previous games are rejected
- **6 languages**: Spanish, English, German, Italian, Portuguese, French

## Trigger command

All languages use the same trigger:

| Language | Trigger |
|----------|---------|
| Español  | `!chess @oponente` |
| English  | `!chess @opponent` |
| Deutsch  | `!chess @gegner` |
| Italiano | `!chess @avversario` |
| Português | `!chess @oponente` |
| Français | `!chess @adversaire` |

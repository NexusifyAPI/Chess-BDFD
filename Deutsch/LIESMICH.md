# Schach — Installationsanleitung

Diese Anleitung erklärt, wie du Schach in Bot Designer For Discord (BDFD) mit Components V2 einrichtest.

## Voraussetzungen

- Ein in BDFD erstellter Bot.
- Components V2 für deinen Bot aktiviert.
- Die Variable `chess_state` angelegt (Standardwert: leer).

## Variablen, die du anlegen musst

Gehe im BDFD-Dashboard auf **Variables** und lege an:

| Name | Wert |
|------|------|
| `chess_state` | (leer lassen) |

Diese Variable speichert den kompletten Spielzustand pro Partie: `fen|whiteID|blackID|gameID|status|lastMove|lastMoveSAN|theme`.

## Externe API

Dieses Spiel benötigt eine zustandslose HTTP-API zur Zugvalidierung und Brett-Renderierung. Die API ist `http://chess.nexusify.co:26102`.

## Installation

### 1. Variable anlegen

Lege `chess_state` mit leerem Standardwert an.

### 2. Die drei Befehle erstellen

Du musst 3 Befehle in BDFD anlegen:

1. **Hauptbefehl** — siehe [`Schachbefehl.md`](./Schachbefehl.md)
2. **Rückruf 1** — siehe [`Rückruf 1 ($onInteraction).md`](./R%C3%BCckruf%201%20%28$onInteraction%29.md)
3. **Rückruf 2** — siehe [`Rückruf 2 ($onInteraction).md`](./R%C3%BCckruf%202%20%28$onInteraction%29.md)

Kopiere für jeden Befehl den Auslöser und den Code aus der jeweiligen Datei.

### 3. Spielen!

Tippe `!chess @gegner` in einem Kanal, in dem dein Bot Nachrichten lesen kann.

## Hinweise

- Der Spielzustand wird pro Benutzer (Herausforderer) gespeichert, sodass mehrere gleichzeitig spielen können.
- Jede Partie hat eine eindeutige Game-ID. Wenn du eine neue Partie startest, funktionieren die Buttons der alten nicht mehr.
- Der Herausforderer kann das Brett-Thema (Green, Blue, Brown, Purple) wählen, bevor der Gegner annimmt.
- Verwende 🏳️ Aufgeben, um aufzugeben, oder 🤝 Remis bieten, um ein Unentschieden vorzuschlagen.

## Slash-Befehl (optional)

Wenn du `/chess` zusätzlich zu `!chess` verwenden möchtest, konfiguriere den Slash-Befehl in BDFD:

| Feld | Wert |
|------|------|
| Option name | `gegner` |
| Option type | User |
| Option Required | Ja |

Nach der Konfiguration können Nutzer `/chess gegner:@gegner` verwenden.

# Scacchi — Guida all'installazione

Questa guida spiega come installare e configurare il gioco di Scacchi in Bot Designer For Discord (BDFD) utilizzando Components V2.

## Prerequisiti

- Un bot creato in BDFD.
- Components V2 abilitato per il tuo bot.
- La variabile `chess_state` creata (valore predefinito: vuoto).

## Variabili da creare

Nel pannello BDFD vai su **Variables** e crea:

| Nome | Valore |
|------|--------|
| `chess_state` | (vuoto) |

Questa variabile memorizza l'intero stato di ogni partita: `fen|whiteID|blackID|gameID|status|lastMove|lastMoveSAN|theme`.

## API esterna

Questo gioco richiede un'API HTTP stateless per validare le mosse e renderizzare la scacchiera. L'API utilizzata è `http://chess.nexusify.co:26102`.

## Installazione

### 1. Creare la variabile

Crea `chess_state` con valore vuoto.

### 2. Creare i tre comandi

Devi creare 3 comandi in BDFD:

1. **Comando principale** — vedi [`Comando_degli_Scacchi.md`](./Comando_degli_Scacchi.md)
2. **Callback 1** — vedi [`Callback 1 ($onInteraction).md`](./Callback%201%20%28$onInteraction%29.md)
3. **Callback 2** — vedi [`Callback 2 ($onInteraction).md`](./Callback%202%20%28$onInteraction%29.md)

Per ogni comando, copia il trigger e il codice dal rispettivo file.

### 3. Gioca!

Scrivi `!chess @avversario` in qualsiasi canale in cui il tuo bot può leggere i messaggi.

## Note

- Lo stato della partita è salvato per utente (sfidante), quindi più utenti possono giocare contemporaneamente.
- Ogni partita ha un ID univoco. Se ne inizi una nuova, i pulsanti della precedente smettono di funzionare.
- Lo sfidante può scegliere il tema della scacchiera (Green, Blue, Brown, Purple) prima che l'avversario accetti.
- Usa 🏳️ Arrenditi per arrenderti, o 🤝 Offri patta per proporre un pareggio.

## Comando Slash (opzionale)

Se vuoi usare `/chess` oltre a `!chess`, configura il comando slash in BDFD:

| Campo | Valore |
|-------|--------|
| Option name | `avversario` |
| Option type | User |
| Option Required | Sì |

Una volta configurato, gli utenti potranno usare `/chess avversario:@avversario`.

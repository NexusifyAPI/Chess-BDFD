# Ajedrez — Guía de instalación

Esta guía explica cómo instalar y configurar el Ajedrez en Bot Designer For Discord (BDFD) usando Components V2.

## Requisitos previos

- Un bot creado en BDFD.
- Components V2 habilitado para tu bot.
- La variable `chess_state` creada (valor por defecto: vacío).

## Variables que debes crear

Entra al panel de BDFD → **Variables** y crea:

| Nombre | Valor |
|--------|-------|
| `chess_state` | (dejar vacío) |

Esta variable guarda el estado completo de cada partida: `fen|whiteID|blackID|gameID|status|lastMove|lastMoveSAN|theme`.

## API externa

Este juego requiere una API HTTP stateless para validar jugadas y renderizar el tablero. La API utilizada es `http://chess.nexusify.co`.

## Instalación

### 1. Crear la variable

Crea `chess_state` con valor vacío.

### 2. Crear los tres comandos

Necesitas crear 3 comandos en BDFD:

1. **Comando principal** — ver [`Comando_de_Ajedrez.md`](./Comando_de_Ajadrez.md)
2. **Callback 1** — ver [`Callback 1 ($onInteraction).md`](./Callback%201%20%28$onInteraction%29.md)
3. **Callback 2** — ver [`Callback 2 ($onInteraction).md`](./Callback%202%20%28$onInteraction%29.md)

Para cada comando, copia el disparador y el código desde el archivo correspondiente.

### 3. ¡A jugar!

Ejecuta `!chess @oponente` en cualquier canal donde tu bot pueda leer mensajes.

## Notas

- El estado se guarda por usuario (retador), así que varios pueden jugar simultáneamente.
- Cada partida tiene un ID único. Si empezás una nueva, los botones de la anterior dejan de funcionar.
- El retador puede elegir el tema del tablero (Green, Blue, Brown, Purple) antes de que el oponente acepte.
- Usá el botón 🏳️ Rendirse para abandonar, o 🤝 Ofrecer tablas para proponer empate.

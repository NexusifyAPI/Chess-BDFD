# Xadrez — Guia de instalação

Este guia explica como instalar e configurar o Xadrez no Bot Designer For Discord (BDFD) usando Components V2.

## Pré-requisitos

- Um bot criado no BDFD.
- Components V2 habilitado para o seu bot.
- A variável `chess_state` criada (valor padrão: vazio).

## Variáveis que você deve criar

No painel do BDFD, vá em **Variables** e crie:

| Nome | Valor |
|------|-------|
| `chess_state` | (deixar vazio) |

Essa variável armazena o estado completo de cada partida: `fen|whiteID|blackID|gameID|status|lastMove|lastMoveSAN|theme`.

## API externa

Este jogo requer uma API HTTP stateless para validar lances e renderizar o tabuleiro. A API utilizada é `http://chess.nexusify.co`.

## Instalação

### 1. Criar a variável

Crie `chess_state` com valor vazio.

### 2. Criar os três comandos

Você precisa criar 3 comandos no BDFD:

1. **Comando principal** — veja [`Comando_de_Xadrez.md`](./Comando_de_Xadrez.md)
2. **Callback 1** — veja [`Callback 1 ($onInteraction).md`](./Callback%201%20%28$onInteraction%29.md)
3. **Callback 2** — veja [`Callback 2 ($onInteraction).md`](./Callback%202%20%28$onInteraction%29.md)

Para cada comando, copie o gatilho e o código do arquivo correspondente.

### 3. Jogar!

Digite `!chess @oponente` em qualquer canal onde seu bot consiga ler mensagens.

## Observações

- O estado da partida é armazenado por usuário (desafiante), então vários jogadores podem jogar simultaneamente.
- Cada partida tem um ID único. Se você iniciar uma nova, os botões da anterior param de funcionar.
- O desafiante pode escolher o tema do tabuleiro (Green, Blue, Brown, Purple) antes de o oponente aceitar.
- Use 🏳️ Render-se para abandonar, ou 🤝 Oferecer empate para propor empate.

## Comando Slash (opcional)

Se você quer usar `/chess` além de `!chess`, configure o comando slash no BDFD:

| Campo | Valor |
|-------|-------|
| Option name | `oponente` |
| Option type | User |
| Option Required | Sim |

Após configurar, os usuários poderão usar `/chess oponente:@oponente`.

# Gatilho:

```
!chess
```

# Código:

```
$nomention
$disableInnerSpaceRemoval

$var[challengerID;$authorID]
$if[$isSlash==true]
$var[opponentID;$message[1;oponente]]
$else
$var[opponentID;$mentioned[1;no]]
$if[$var[opponentID]==]
$addTextDisplay[❌ Você deve mencionar um adversário. Uso: \`$commandTrigger @usuário\`]
$stop
$endif
$endif

$if[$var[opponentID]==$var[challengerID]]
$ephemeral
$addTextDisplay[❌ Você não pode se desafiar.]
$stop
$endif

$var[existingState;$getVar[chess_state;$var[challengerID]]]
$if[$var[existingState]!=]
$textSplit[$var[existingState];|]
$var[existingStatus;$splitText[5]]
$if[$var[existingStatus]==p]
$ephemeral
$addTextDisplay[❌ Você já tem uma partida em andamento. Termine-a ou renda-se antes de criar outra.]
$stop
$endif
$if[$var[existingStatus]==c]
$ephemeral
$addTextDisplay[❌ Você já tem um desafio pendente. Cancele-o antes de criar outro.]
$stop
$endif
$endif

$var[gameID;$randomString[10]]
$var[theme;green]
$var[state;rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1|$var[challengerID]|$var[opponentID]|$var[gameID]|c|-|-|$var[theme]]
$setVar[chess_state;$var[state];$var[challengerID]]

$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Xadrez — Desafio pendente**

<@$var[challengerID]> (♔ Brancas) desafia <@$var[opponentID]> (♚ Pretas) para uma partida de xadrez.

⏳ Aguardando resposta de <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Aceitar;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Recusar;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Cancelar;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**ID da partida:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Escolher tabuleiro** — Apenas <@$var[challengerID]> pode mudar o tema.
Tema atual: **$var[theme]**;themes]
$addSeparator[true;small;themes]
$addActionRow[tr1;themes]
$var[tgStyle;secondary]
$var[tgDis;false]
$if[$var[theme]==green]
$var[tgStyle;success]
$var[tgDis;true]
$endif
$addButtonCV2[chthemegr~$var[challengerID]~$var[gameID];🟢 Green;$var[tgStyle];$var[tgDis];;tr1]
$var[tbStyle;secondary]
$var[tbDis;false]
$if[$var[theme]==blue]
$var[tbStyle;success]
$var[tbDis;true]
$endif
$addButtonCV2[chthemebl~$var[challengerID]~$var[gameID];🔵 Blue;$var[tbStyle];$var[tbDis];;tr1]
$var[twStyle;secondary]
$var[twDis;false]
$if[$var[theme]==brown]
$var[twStyle;success]
$var[twDis;true]
$endif
$addButtonCV2[chthemebr~$var[challengerID]~$var[gameID];🟤 Brown;$var[twStyle];$var[twDis];;tr1]
$var[tpStyle;secondary]
$var[tpDis;false]
$if[$var[theme]==purple]
$var[tpStyle;success]
$var[tpDis;true]
$endif
$addButtonCV2[chthemepu~$var[challengerID]~$var[gameID];🟣 Purple;$var[tpStyle];$var[tpDis];;tr1]
```

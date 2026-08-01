# Disparador:

```
!chess
```

# Código:

```
$nomention
$disableInnerSpaceRemoval
$try

$var[challengerID;$authorID]
$if[$isSlash==true]
$var[opponentID;$message[1;usuario]]
$else
$var[opponentID;$mentioned[1;no]]
$if[$var[opponentID]==]
$addTextDisplay[❌ Debes mencionar a un oponente. Uso: \`$commandTrigger @usuario\`]
$stop
$endif
$endif

$if[$var[opponentID]==$var[challengerID]]
$ephemeral
$addTextDisplay[❌ No puedes desafiarte a ti mismo.]
$stop
$endif

$var[existingState;$getVar[chess_state;$var[challengerID]]]
$if[$var[existingState]!=]
$textSplit[$var[existingState];|]
$var[existingStatus;$splitText[5]]
$if[$var[existingStatus]==p]
$var[oldOpponent;$splitText[3]]
$var[oldGameID;$splitText[4]]
$ephemeral
$addContainer[errbox;#ED4245;false]
$addTextDisplay[❌ Ya tienes una partida en curso. Cancela la partida anterior para crear una nueva.;errbox]
$addSeparator[true;small;errbox]
$addActionRow[forcebtn;errbox]
$addButtonCV2[chforce~$var[challengerID]~$var[opponentID]~$var[oldOpponent]~$var[oldGameID];🗑️ Cancelar partida anterior;danger;false;;forcebtn]
$stop
$endif
$if[$var[existingStatus]==c]
$var[oldOpponent;$splitText[3]]
$var[oldGameID;$splitText[4]]
$ephemeral
$addContainer[errbox;#ED4245;false]
$addTextDisplay[❌ Ya tienes un reto pendiente. Cancela el reto anterior para crear uno nuevo.;errbox]
$addSeparator[true;small;errbox]
$addActionRow[forcebtn;errbox]
$addButtonCV2[chforce~$var[challengerID]~$var[opponentID]~$var[oldOpponent]~$var[oldGameID];🗑️ Cancelar reto anterior;danger;false;;forcebtn]
$stop
$endif
$endif

$var[gameID;$randomString[10]]
$var[theme;green]
$var[trigger;$commandTrigger]
$var[state;rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1|$var[challengerID]|$var[opponentID]|$var[gameID]|c|-|-|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[state];$var[challengerID]]

$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Ajedrez — Reto pendiente**

<@$var[challengerID]> (♔ Blancas) desafía a <@$var[opponentID]> (♚ Negras) a una partida de ajedrez.

⏳ Esperando respuesta de <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Aceptar;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Rechazar;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Cancelar;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**ID de partida:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Elegir tablero** — Solo <@$var[challengerID]> puede cambiar el tema.
Tema actual: **$var[theme]**;themes]
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
$catch
$ephemeral
$addTextDisplay[❌ Ocurrió un error inesperado: $error[message]]
$endtry
```

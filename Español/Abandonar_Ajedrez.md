# Disparador:

```
!leavechess
```

# Código:

```
$nomention
$disableInnerSpaceRemoval
$try

$var[myState;$getVar[chess_state;$authorID]]
$if[$var[myState]==]
$addTextDisplay[ℹ️ No tienes ninguna partida activa.]
$stop
$endif

$textSplit[$var[myState];|]
$var[oldStatus;$splitText[5]]
$var[opponentID;$splitText[3]]

$setVar[chess_state;;$authorID]

$var[statusMsg;Partida abandonada.]
$if[$var[oldStatus]==c]
$var[statusMsg;Reto cancelado.]
$endif
$if[$var[oldStatus]==p]
$var[statusMsg;Partida abandonada. <@$var[opponentID]> ya puede jugar de nuevo.]
$endif

$addContainer[main;#2B2D31;false]
$addTextDisplay[🚪 **Ajedrez — $var[statusMsg]**

Ya podés crear una nueva partida con \`!chess\`.;main]

$catch
$addTextDisplay[❌ Ocurrió un error inesperado: $error[message]]
$endtry
```

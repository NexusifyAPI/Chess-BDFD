# Gatilho:

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
$addTextDisplay[ℹ️ Você não tem nenhuma partida ativa.]
$stop
$endif

$textSplit[$var[myState];|]
$var[oldStatus;$splitText[5]]
$var[opponentID;$splitText[3]]

$setVar[chess_state;;$authorID]

$var[statusMsg;Partida abandonada.]
$if[$var[oldStatus]==c]
$var[statusMsg;Desafio cancelado.]
$endif
$if[$var[oldStatus]==p]
$var[statusMsg;Partida abandonada. <@$var[opponentID]> já pode jogar de novo.]
$endif

$addContainer[main;#2B2D31;false]
$addTextDisplay[🚪 **Xadrez — $var[statusMsg]**

Você já pode criar uma nova partida com \`!chess\`.;main]

$catch
$addTextDisplay[❌ Ocorreu um erro inesperado: $error[message]]
$endtry
```

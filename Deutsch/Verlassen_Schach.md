# Auslöser:

```
!leavechess
```

# Code:

```
$nomention
$disableInnerSpaceRemoval
$try

$var[myState;$getVar[chess_state;$authorID]]
$if[$var[myState]==]
$addTextDisplay[ℹ️ Du hast kein aktives Spiel.]
$stop
$endif

$textSplit[$var[myState];|]
$var[oldStatus;$splitText[5]]
$var[opponentID;$splitText[3]]

$setVar[chess_state;;$authorID]

$var[statusMsg;Partie verlassen.]
$if[$var[oldStatus]==c]
$var[statusMsg;Herausforderung abgebrochen.]
$endif
$if[$var[oldStatus]==p]
$var[statusMsg;Partie verlassen. <@$var[opponentID]> kann wieder spielen.]
$endif

$addContainer[main;#2B2D31;false]
$addTextDisplay[🚪 **Schach — $var[statusMsg]**

Du kannst jetzt ein neues Spiel mit \`!chess\` erstellen.;main]

$catch
$addTextDisplay[❌ Ein unerwarteter Fehler ist aufgetreten: $error[message]]
$endtry
```

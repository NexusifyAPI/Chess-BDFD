# Trigger:

```
!leavechess
```

# Codice:

```
$nomention
$disableInnerSpaceRemoval
$try

$var[myState;$getVar[chess_state;$authorID]]
$if[$var[myState]==]
$addTextDisplay[ℹ️ Non hai nessuna partita attiva.]
$stop
$endif

$textSplit[$var[myState];|]
$var[oldStatus;$splitText[5]]
$var[opponentID;$splitText[3]]

$setVar[chess_state;;$authorID]

$var[statusMsg;Partita abbandonata.]
$if[$var[oldStatus]==c]
$var[statusMsg;Sfida annullata.]
$endif
$if[$var[oldStatus]==p]
$var[statusMsg;Partita abbandonata. <@$var[opponentID]> può giocare di nuovo.]
$endif

$addContainer[main;#2B2D31;false]
$addTextDisplay[🚪 **Scacchi — $var[statusMsg]**

Puoi già creare una nuova partita con \`!chess\`.;main]

$catch
$addTextDisplay[❌ Si è verificato un errore imprevisto: $error[message]]
$endtry
```

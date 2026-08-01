# Trigger:

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
$addTextDisplay[ℹ️ You don't have any active game.]
$stop
$endif

$textSplit[$var[myState];|]
$var[oldStatus;$splitText[5]]
$var[opponentID;$splitText[3]]

$setVar[chess_state;;$authorID]

$var[statusMsg;Game abandoned.]
$if[$var[oldStatus]==c]
$var[statusMsg;Challenge cancelled.]
$endif
$if[$var[oldStatus]==p]
$var[statusMsg;Game abandoned. <@$var[opponentID]> ya puede jugar de nuevo.]
$endif

$addContainer[main;#2B2D31;false]
$addTextDisplay[🚪 **Chess — $var[statusMsg]**

You can now create a new game with \`!chess\`.;main]

$catch
$addTextDisplay[❌ Ocurrió un error: $error[message]]
$endtry
```

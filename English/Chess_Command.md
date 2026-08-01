# Trigger:

```
!chess
```

# Code:

```
$nomention
$disableInnerSpaceRemoval
$try

$var[challengerID;$authorID]
$if[$isSlash==true]
$var[opponentID;$message[1;user]]
$else
$var[opponentID;$mentioned[1;no]]
$if[$var[opponentID]==]
$addTextDisplay[❌ You must mention an opponent. Usage: \`$commandTrigger @user\`]
$stop
$endif
$endif

$if[$var[opponentID]==$var[challengerID]]
$ephemeral
$addTextDisplay[❌ You cannot challenge yourself.]
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
$addTextDisplay[❌ You already have an ongoing game. Cancela la partida anterior para crear una nueva.;errbox]
$addSeparator[true;small;errbox]
$addActionRow[forcebtn;errbox]
$addButtonCV2[chforce~$var[challengerID]~$var[opponentID]~$var[oldOpponent]~$var[oldGameID];🗑️ Cancel previous game;danger;false;;forcebtn]
$stop
$endif
$if[$var[existingStatus]==c]
$var[oldOpponent;$splitText[3]]
$var[oldGameID;$splitText[4]]
$ephemeral
$addContainer[errbox;#ED4245;false]
$addTextDisplay[❌ You already have a pending challenge. Cancela el reto anterior para crear uno nuevo.;errbox]
$addSeparator[true;small;errbox]
$addActionRow[forcebtn;errbox]
$addButtonCV2[chforce~$var[challengerID]~$var[opponentID]~$var[oldOpponent]~$var[oldGameID];🗑️ Cancel previous challenge;danger;false;;forcebtn]
$stop
$endif
$endif

$var[gameID;$randomString[10]]
$var[theme;green]
$var[state;rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1|$var[challengerID]|$var[opponentID]|$var[gameID]|c|-|-|$var[theme]]
$setVar[chess_state;$var[state];$var[challengerID]]

$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Chess — Pending challenge**

<@$var[challengerID]> (♔ White) challenges <@$var[opponentID]> (♚ Black) to a chess match.

⏳ Waiting for response from <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Accept;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Decline;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Cancel;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**Game ID:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Choose board** — Only <@$var[challengerID]> can change the theme.
Current theme: **$var[theme]**;themes]
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
$addTextDisplay[❌ An unexpected error occurred: $error[message]]
$endtry
```

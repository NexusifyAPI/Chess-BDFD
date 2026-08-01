# Déclencheur:

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
$addTextDisplay[ℹ️ Tu n'as aucune partie active.]
$stop
$endif

$textSplit[$var[myState];|]
$var[oldStatus;$splitText[5]]
$var[opponentID;$splitText[3]]

$setVar[chess_state;;$authorID]

$var[statusMsg;Partie abandonnée.]
$if[$var[oldStatus]==c]
$var[statusMsg;Défi annulé.]
$endif
$if[$var[oldStatus]==p]
$var[statusMsg;Partie abandonnée. <@$var[opponentID]> peut rejouer.]
$endif

$addContainer[main;#2B2D31;false]
$addTextDisplay[🚪 **Échecs — $var[statusMsg]**

Tu peux maintenant créer une nouvelle partie avec \`!chess\`.;main]

$catch
$addTextDisplay[❌ Ocurrió un error: $error[message]]
$endtry
```

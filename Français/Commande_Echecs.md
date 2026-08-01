# Déclencheur:

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
$var[opponentID;$message[1;adversaire]]
$else
$var[opponentID;$mentioned[1;no]]
$if[$var[opponentID]==]
$addTextDisplay[❌ Vous devez mentionner un adversaire. Utilisation : \`$commandTrigger @utilisateur\`]
$stop
$endif
$endif

$if[$var[opponentID]==$var[challengerID]]
$ephemeral
$addTextDisplay[❌ Vous ne pouvez pas vous défier vous-même.]
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
$addTextDisplay[❌ Vous avez déjà une partie en cours. Annulez la partie précédente pour en créer une nouvelle.;errbox]
$addSeparator[true;small;errbox]
$addActionRow[forcebtn;errbox]
$addButtonCV2[chforce~$var[challengerID]~$var[opponentID]~$var[oldOpponent]~$var[oldGameID];🗑️ Annuler la partie précédente;danger;false;;forcebtn]
$stop
$endif
$if[$var[existingStatus]==c]
$var[oldOpponent;$splitText[3]]
$var[oldGameID;$splitText[4]]
$ephemeral
$addContainer[errbox;#ED4245;false]
$addTextDisplay[❌ Vous avez déjà un défi en attente. Annulez le défi précédent pour en créer un nouveau.;errbox]
$addSeparator[true;small;errbox]
$addActionRow[forcebtn;errbox]
$addButtonCV2[chforce~$var[challengerID]~$var[opponentID]~$var[oldOpponent]~$var[oldGameID];🗑️ Annuler le défi précédent;danger;false;;forcebtn]
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
$addTextDisplay[# ♟️ **Échecs — Défi en attente**

<@$var[challengerID]> (♔ Blancs) défie <@$var[opponentID]> (♚ Noirs) à une partie d'échecs.

⏳ En attente de la réponse de <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Accepter;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Refuser;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Annuler;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**ID de partie :** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Choisir l'échiquier** — Seul <@$var[challengerID]> peut changer le thème.
Thème actuel : **$var[theme]**;themes]
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
$addTextDisplay[❌ Une erreur inattendue s'est produite : $error[message]]
$endtry
```

# Trigger:

```
!chess
```

# Codice:

```
$nomention
$disableInnerSpaceRemoval
$try

$var[challengerID;$authorID]
$if[$isSlash==true]
$var[opponentID;$message[1;avversario]]
$else
$var[opponentID;$mentioned[1;no]]
$if[$var[opponentID]==]
$addTextDisplay[❌ Devi menzionare un avversario. Uso: \`$commandTrigger @utente\`]
$stop
$endif
$endif

$if[$var[opponentID]==$var[challengerID]]
$ephemeral
$addTextDisplay[❌ Non puoi sfidare te stesso.]
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
$addTextDisplay[❌ Hai già una partita in corso. Cancela la partida anterior para crear una nueva.;errbox]
$addSeparator[true;small;errbox]
$addActionRow[forcebtn;errbox]
$addButtonCV2[chforce~$var[challengerID]~$var[opponentID]~$var[oldOpponent]~$var[oldGameID];🗑️ Annulla partita precedente;danger;false;;forcebtn]
$stop
$endif
$if[$var[existingStatus]==c]
$var[oldOpponent;$splitText[3]]
$var[oldGameID;$splitText[4]]
$ephemeral
$addContainer[errbox;#ED4245;false]
$addTextDisplay[❌ Hai già una sfida in attesa. Cancela el reto anterior para crear uno nuevo.;errbox]
$addSeparator[true;small;errbox]
$addActionRow[forcebtn;errbox]
$addButtonCV2[chforce~$var[challengerID]~$var[opponentID]~$var[oldOpponent]~$var[oldGameID];🗑️ Annulla sfida precedente;danger;false;;forcebtn]
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
$addTextDisplay[# ♟️ **Scacchi — sfida in attesa**

<@$var[challengerID]> (♔ Bianchi) sfida <@$var[opponentID]> (♚ Neri) a una partita a scacchi.

⏳ In attesa di risposta da <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Accetta;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Rifiuta;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Annulla;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**ID partita:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Scegli scacchiera** — Solo <@$var[challengerID]> può cambiare il tema.
Tema attuale: **$var[theme]**;themes]
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
$addTextDisplay[❌ Si è verificato un errore imprevisto: $error[message]]
$endtry
```

# Auslöser:

```
$onInteraction
```

# Code:

```
$nomention
$disableInnerSpaceRemoval
$try

$if[$checkContains[$customID;chaccept~;chdecline~;chcancel~;chnew~;chclose~;chbp~;chforfeit~;chdraw~;chdrawdecline~;chthemegr~;chthemebl~;chthemebr~;chthemepu~;chforce~]==false]
$stop
$endif

$textSplit[$customID;~]
$var[action;$splitText[1]]

$if[$var[action]==chaccept]
$var[wID;$splitText[2]]
$var[bID;$splitText[3]]
$var[gID;$splitText[4]]

$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ Nur <@$var[bID]> kann diese Herausforderung annehmen.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Diese Herausforderung ist nicht mehr aktiv. Führe \`$var[trigger]\` erneut aus.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]
$var[theme;$splitText[8]]
$if[$var[theme]==]
$var[theme;green]
$endif

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Diese Herausforderung ist nicht mehr aktiv.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[❌ Diese Herausforderung wurde bereits beantwortet.]
$stop
$endif

$defer
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|p|-|-|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[lastMoveSAN;-]
$removeAllComponents
$httpGet[http://chess.nexusify.co/moves?fen=$url[encode;$var[fen]]]
$var[movesCount;$httpResult[count]]
$var[turn;$httpResult[turn]]
$var[cc;#5865F2]
$var[turnLabel;♔ Weiß]
$var[activePlayer;$var[wID]]
$var[perspective;white]
$if[$var[turn]==b]
$var[turnLabel;♚ Schwarz]
$var[activePlayer;$var[bID]]
$endif
$var[lblP;♙ Bauer]$var[lblN;♘ Springer]$var[lblB;♗ Läufer]$var[lblR;♖ Turm]$var[lblQ;♕ Dame]$var[lblK;♔ König]$if[$var[turn]==b]$var[lblP;♟ Bauer]$var[lblN;♞ Springer]$var[lblB;♝ Läufer]$var[lblR;♜ Turm]$var[lblQ;♛ Dame]$var[lblK;♚ König]$endif
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Schach**

**Zug:** $var[turnLabel]
**Am Zug:** <@$var[activePlayer]>
**Letzter Zug:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Schachbrett;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Dein Zug, <@$var[activePlayer]>.** Wähle eine Figur zum Ziehen:;main]
$addActionRow[r1;main]
$addStringSelect[chpiece~$var[wID]~$var[gID];Figur wählen...;1;1;;r1]
$if[$httpResult[moves_by_square;0;from]!=]$var[p0;$httpResult[moves_by_square;0;piece]]$var[from0;$httpResult[moves_by_square;0;from]]$var[lbl0;$var[lblP]]$if[$var[p0]==N]$var[lbl0;$var[lblN]]$endif$if[$var[p0]==B]$var[lbl0;$var[lblB]]$endif$if[$var[p0]==R]$var[lbl0;$var[lblR]]$endif$if[$var[p0]==Q]$var[lbl0;$var[lblQ]]$endif$if[$var[p0]==K]$var[lbl0;$var[lblK]]$endif$addStringSelectOption[$var[lbl0] auf $var[from0];$var[from0];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;1;from]!=]$var[p1;$httpResult[moves_by_square;1;piece]]$var[from1;$httpResult[moves_by_square;1;from]]$var[lbl1;$var[lblP]]$if[$var[p1]==N]$var[lbl1;$var[lblN]]$endif$if[$var[p1]==B]$var[lbl1;$var[lblB]]$endif$if[$var[p1]==R]$var[lbl1;$var[lblR]]$endif$if[$var[p1]==Q]$var[lbl1;$var[lblQ]]$endif$if[$var[p1]==K]$var[lbl1;$var[lblK]]$endif$addStringSelectOption[$var[lbl1] auf $var[from1];$var[from1];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;2;from]!=]$var[p2;$httpResult[moves_by_square;2;piece]]$var[from2;$httpResult[moves_by_square;2;from]]$var[lbl2;$var[lblP]]$if[$var[p2]==N]$var[lbl2;$var[lblN]]$endif$if[$var[p2]==B]$var[lbl2;$var[lblB]]$endif$if[$var[p2]==R]$var[lbl2;$var[lblR]]$endif$if[$var[p2]==Q]$var[lbl2;$var[lblQ]]$endif$if[$var[p2]==K]$var[lbl2;$var[lblK]]$endif$addStringSelectOption[$var[lbl2] auf $var[from2];$var[from2];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;3;from]!=]$var[p3;$httpResult[moves_by_square;3;piece]]$var[from3;$httpResult[moves_by_square;3;from]]$var[lbl3;$var[lblP]]$if[$var[p3]==N]$var[lbl3;$var[lblN]]$endif$if[$var[p3]==B]$var[lbl3;$var[lblB]]$endif$if[$var[p3]==R]$var[lbl3;$var[lblR]]$endif$if[$var[p3]==Q]$var[lbl3;$var[lblQ]]$endif$if[$var[p3]==K]$var[lbl3;$var[lblK]]$endif$addStringSelectOption[$var[lbl3] auf $var[from3];$var[from3];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;4;from]!=]$var[p4;$httpResult[moves_by_square;4;piece]]$var[from4;$httpResult[moves_by_square;4;from]]$var[lbl4;$var[lblP]]$if[$var[p4]==N]$var[lbl4;$var[lblN]]$endif$if[$var[p4]==B]$var[lbl4;$var[lblB]]$endif$if[$var[p4]==R]$var[lbl4;$var[lblR]]$endif$if[$var[p4]==Q]$var[lbl4;$var[lblQ]]$endif$if[$var[p4]==K]$var[lbl4;$var[lblK]]$endif$addStringSelectOption[$var[lbl4] auf $var[from4];$var[from4];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;5;from]!=]$var[p5;$httpResult[moves_by_square;5;piece]]$var[from5;$httpResult[moves_by_square;5;from]]$var[lbl5;$var[lblP]]$if[$var[p5]==N]$var[lbl5;$var[lblN]]$endif$if[$var[p5]==B]$var[lbl5;$var[lblB]]$endif$if[$var[p5]==R]$var[lbl5;$var[lblR]]$endif$if[$var[p5]==Q]$var[lbl5;$var[lblQ]]$endif$if[$var[p5]==K]$var[lbl5;$var[lblK]]$endif$addStringSelectOption[$var[lbl5] auf $var[from5];$var[from5];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;6;from]!=]$var[p6;$httpResult[moves_by_square;6;piece]]$var[from6;$httpResult[moves_by_square;6;from]]$var[lbl6;$var[lblP]]$if[$var[p6]==N]$var[lbl6;$var[lblN]]$endif$if[$var[p6]==B]$var[lbl6;$var[lblB]]$endif$if[$var[p6]==R]$var[lbl6;$var[lblR]]$endif$if[$var[p6]==Q]$var[lbl6;$var[lblQ]]$endif$if[$var[p6]==K]$var[lbl6;$var[lblK]]$endif$addStringSelectOption[$var[lbl6] auf $var[from6];$var[from6];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;7;from]!=]$var[p7;$httpResult[moves_by_square;7;piece]]$var[from7;$httpResult[moves_by_square;7;from]]$var[lbl7;$var[lblP]]$if[$var[p7]==N]$var[lbl7;$var[lblN]]$endif$if[$var[p7]==B]$var[lbl7;$var[lblB]]$endif$if[$var[p7]==R]$var[lbl7;$var[lblR]]$endif$if[$var[p7]==Q]$var[lbl7;$var[lblQ]]$endif$if[$var[p7]==K]$var[lbl7;$var[lblK]]$endif$addStringSelectOption[$var[lbl7] auf $var[from7];$var[from7];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;8;from]!=]$var[p8;$httpResult[moves_by_square;8;piece]]$var[from8;$httpResult[moves_by_square;8;from]]$var[lbl8;$var[lblP]]$if[$var[p8]==N]$var[lbl8;$var[lblN]]$endif$if[$var[p8]==B]$var[lbl8;$var[lblB]]$endif$if[$var[p8]==R]$var[lbl8;$var[lblR]]$endif$if[$var[p8]==Q]$var[lbl8;$var[lblQ]]$endif$if[$var[p8]==K]$var[lbl8;$var[lblK]]$endif$addStringSelectOption[$var[lbl8] auf $var[from8];$var[from8];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;9;from]!=]$var[p9;$httpResult[moves_by_square;9;piece]]$var[from9;$httpResult[moves_by_square;9;from]]$var[lbl9;$var[lblP]]$if[$var[p9]==N]$var[lbl9;$var[lblN]]$endif$if[$var[p9]==B]$var[lbl9;$var[lblB]]$endif$if[$var[p9]==R]$var[lbl9;$var[lblR]]$endif$if[$var[p9]==Q]$var[lbl9;$var[lblQ]]$endif$if[$var[p9]==K]$var[lbl9;$var[lblK]]$endif$addStringSelectOption[$var[lbl9] auf $var[from9];$var[from9];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;10;from]!=]$var[p10;$httpResult[moves_by_square;10;piece]]$var[from10;$httpResult[moves_by_square;10;from]]$var[lbl10;$var[lblP]]$if[$var[p10]==N]$var[lbl10;$var[lblN]]$endif$if[$var[p10]==B]$var[lbl10;$var[lblB]]$endif$if[$var[p10]==R]$var[lbl10;$var[lblR]]$endif$if[$var[p10]==Q]$var[lbl10;$var[lblQ]]$endif$if[$var[p10]==K]$var[lbl10;$var[lblK]]$endif$addStringSelectOption[$var[lbl10] auf $var[from10];$var[from10];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;11;from]!=]$var[p11;$httpResult[moves_by_square;11;piece]]$var[from11;$httpResult[moves_by_square;11;from]]$var[lbl11;$var[lblP]]$if[$var[p11]==N]$var[lbl11;$var[lblN]]$endif$if[$var[p11]==B]$var[lbl11;$var[lblB]]$endif$if[$var[p11]==R]$var[lbl11;$var[lblR]]$endif$if[$var[p11]==Q]$var[lbl11;$var[lblQ]]$endif$if[$var[p11]==K]$var[lbl11;$var[lblK]]$endif$addStringSelectOption[$var[lbl11] auf $var[from11];$var[from11];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;12;from]!=]$var[p12;$httpResult[moves_by_square;12;piece]]$var[from12;$httpResult[moves_by_square;12;from]]$var[lbl12;$var[lblP]]$if[$var[p12]==N]$var[lbl12;$var[lblN]]$endif$if[$var[p12]==B]$var[lbl12;$var[lblB]]$endif$if[$var[p12]==R]$var[lbl12;$var[lblR]]$endif$if[$var[p12]==Q]$var[lbl12;$var[lblQ]]$endif$if[$var[p12]==K]$var[lbl12;$var[lblK]]$endif$addStringSelectOption[$var[lbl12] auf $var[from12];$var[from12];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;13;from]!=]$var[p13;$httpResult[moves_by_square;13;piece]]$var[from13;$httpResult[moves_by_square;13;from]]$var[lbl13;$var[lblP]]$if[$var[p13]==N]$var[lbl13;$var[lblN]]$endif$if[$var[p13]==B]$var[lbl13;$var[lblB]]$endif$if[$var[p13]==R]$var[lbl13;$var[lblR]]$endif$if[$var[p13]==Q]$var[lbl13;$var[lblQ]]$endif$if[$var[p13]==K]$var[lbl13;$var[lblK]]$endif$addStringSelectOption[$var[lbl13] auf $var[from13];$var[from13];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;14;from]!=]$var[p14;$httpResult[moves_by_square;14;piece]]$var[from14;$httpResult[moves_by_square;14;from]]$var[lbl14;$var[lblP]]$if[$var[p14]==N]$var[lbl14;$var[lblN]]$endif$if[$var[p14]==B]$var[lbl14;$var[lblB]]$endif$if[$var[p14]==R]$var[lbl14;$var[lblR]]$endif$if[$var[p14]==Q]$var[lbl14;$var[lblQ]]$endif$if[$var[p14]==K]$var[lbl14;$var[lblK]]$endif$addStringSelectOption[$var[lbl14] auf $var[from14];$var[from14];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;15;from]!=]$var[p15;$httpResult[moves_by_square;15;piece]]$var[from15;$httpResult[moves_by_square;15;from]]$var[lbl15;$var[lblP]]$if[$var[p15]==N]$var[lbl15;$var[lblN]]$endif$if[$var[p15]==B]$var[lbl15;$var[lblB]]$endif$if[$var[p15]==R]$var[lbl15;$var[lblR]]$endif$if[$var[p15]==Q]$var[lbl15;$var[lblQ]]$endif$if[$var[p15]==K]$var[lbl15;$var[lblK]]$endif$addStringSelectOption[$var[lbl15] auf $var[from15];$var[from15];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Aufgeben;danger;false;;ctrl]
$addButtonCV2[chdraw~$var[wID]~$var[gID];🤝 Remis bieten;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Spieler:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chdecline]
$var[wID;$splitText[2]]
$var[bID;$splitText[3]]
$var[gID;$splitText[4]]

$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ Nur <@$var[bID]> kann diese Herausforderung ablehnen.]
$stop
$endif

$defer
$setVar[chess_state;;$var[wID]]
$removeAllComponents
$addContainer[main;#ED4245;false]
$addTextDisplay[# ♟️ **Herausforderung abgelehnt**

<@$var[bID]> hat die Herausforderung von <@$var[wID]> abgelehnt.;main]
$stop
$endif

$if[$var[action]==chcancel]
$var[wID;$splitText[2]]
$var[bID;$splitText[3]]
$var[gID;$splitText[4]]

$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Nur <@$var[wID]> kann diese Herausforderung abbrechen.]
$stop
$endif

$defer
$setVar[chess_state;;$var[wID]]
$removeAllComponents
$addContainer[main;#ED4245;false]
$addTextDisplay[# ♟️ **Herausforderung abgebrochen**

<@$var[wID]> hat die Herausforderung abgebrochen.;main]
$stop
$endif

$if[$var[action]==chnew]
$var[oldwID;$splitText[2]]
$var[oldbID;$splitText[3]]

$if[$authorID!=$var[oldwID]]
$if[$authorID!=$var[oldbID]]
$ephemeral
$addTextDisplay[❌ Nur die Spieler dieser Partie können eine neue starten.]
$stop
$endif
$endif

$var[existingState;$getVar[chess_state;$var[oldwID]]]
$if[$var[existingState]!=]
$textSplit[$var[existingState];|]
$var[existingStatus;$splitText[5]]
$if[$var[existingStatus]==p]
$ephemeral
$addTextDisplay[❌ Du hast bereits eine laufende Partie. Brich die vorherige Partie ab, um eine neue zu erstellen.]
$stop
$endif
$if[$var[existingStatus]==c]
$ephemeral
$addTextDisplay[❌ Du hast bereits eine ausstehende Herausforderung. Brich die vorherige ab, um eine neue zu erstellen.]
$stop
$endif
$endif

$defer
$var[gameID;$randomString[10]]
$var[theme;green]
$var[trigger;!chess]
$if[$var[existingState]!=]
$textSplit[$var[existingState];|]
$var[trigger;$splitText[9]]
$if[$var[trigger]==]
$var[trigger;!chess]
$endif
$endif
$var[state;rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1|$var[oldwID]|$var[oldbID]|$var[gameID]|c|-|-|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[state];$var[oldwID]]

$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Schach — Neue Partie**

<@$var[oldwID]> (♔ Weiß) fordert heraus: <@$var[oldbID]> (♚ Schwarz) zu einer neuen Schachpartie.

⏳ Warte auf Antwort von <@$var[oldbID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[oldwID]~$var[oldbID]~$var[gameID];✅ Annehmen;success;false;;r1]
$addButtonCV2[chdecline~$var[oldwID]~$var[oldbID]~$var[gameID];❌ Ablehnen;danger;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**Partie-ID:** \`$var[gameID]\`;main]
$stop
$endif

$if[$var[action]==chclose]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[closeAllowed;no]
$var[closeState;$getVar[chess_state;$var[wID]]]
$if[$var[closeState]!=]
$textSplit[$var[closeState];|]
$var[closeBID;$splitText[3]]
$if[$authorID==$var[wID]]$var[closeAllowed;yes]$endif
$if[$authorID==$var[closeBID]]$var[closeAllowed;yes]$endif
$endif
$if[$var[closeAllowed]!=yes]
$ephemeral
$addTextDisplay[❌ Nur die Spieler dieser Partie können sie schließen.]
$stop
$endif

$defer
$setVar[chess_state;;$var[wID]]
$removeAllComponents
$addContainer[main;#2B2D31;false]
$addTextDisplay[# ♟️ **Schach — Geschlossen**

Die Partie wurde geschlossen.;main]
$stop
$endif

$if[$var[action]==chbp]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Du hast keine aktive Partie. Führe \`$var[trigger]\` aus, um eine zu erstellen.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]
$var[theme;$splitText[8]]
$if[$var[theme]==]
$var[theme;green]
$endif
$var[trigger;$splitText[9]]
$if[$var[trigger]==]
$var[trigger;!chess]
$endif

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Diese Partie ist nicht mehr aktiv. Führe \`$var[trigger]\` aus, um eine neue zu erstellen.]
$stop
$endif

$if[$var[curStatus]==c]
$ephemeral
$addTextDisplay[ℹ️ Die Herausforderung wurde noch nicht angenommen. Warte, bis der Gegner annimmt.]
$stop
$endif

$if[$var[curStatus]==do]
$ephemeral
$addTextDisplay[ℹ️ Es steht ein Remisangebot aus. Antworte zuerst darauf.]
$stop
$endif

$if[$var[curStatus]!=p]
$ephemeral
$addTextDisplay[ℹ️ Diese Partie ist bereits beendet. Verwende 🎲 Neue Partie, um eine andere zu spielen.]
$stop
$endif

$textSplit[$var[fen]; ]
$var[turn;$splitText[2]]
$var[expectedPlayer;$var[wID]]
$if[$var[turn]==b]$var[expectedPlayer;$var[bID]]$endif
$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ Du bist kein Spieler dieser Partie.]
$stop
$endif
$endif
$if[$authorID!=$var[expectedPlayer]]
$ephemeral
$addTextDisplay[❌ Du bist nicht am Zug. Warte, bis der andere Spieler zieht.]
$stop
$endif

$defer
$var[lastMoveSAN;-]
$textSplit[$var[state];|]
$var[lastMoveSAN;$splitText[7]]
$removeAllComponents
$httpGet[http://chess.nexusify.co/moves?fen=$url[encode;$var[fen]]]
$var[movesCount;$httpResult[count]]
$var[turn;$httpResult[turn]]
$var[cc;#5865F2]
$var[turnLabel;♔ Weiß]
$var[activePlayer;$var[wID]]
$var[perspective;white]
$if[$var[turn]==b]
$var[turnLabel;♚ Schwarz]
$var[activePlayer;$var[bID]]
$endif
$var[lblP;♙ Bauer]$var[lblN;♘ Springer]$var[lblB;♗ Läufer]$var[lblR;♖ Turm]$var[lblQ;♕ Dame]$var[lblK;♔ König]$if[$var[turn]==b]$var[lblP;♟ Bauer]$var[lblN;♞ Springer]$var[lblB;♝ Läufer]$var[lblR;♜ Turm]$var[lblQ;♛ Dame]$var[lblK;♚ König]$endif
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Schach**

**Zug:** $var[turnLabel]
**Am Zug:** <@$var[activePlayer]>
**Letzter Zug:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Schachbrett;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Dein Zug, <@$var[activePlayer]>.** Wähle eine Figur zum Ziehen:;main]
$addActionRow[r1;main]
$addStringSelect[chpiece~$var[wID]~$var[gID];Figur wählen...;1;1;;r1]
$if[$httpResult[moves_by_square;0;from]!=]$var[p0;$httpResult[moves_by_square;0;piece]]$var[from0;$httpResult[moves_by_square;0;from]]$var[lbl0;$var[lblP]]$if[$var[p0]==N]$var[lbl0;$var[lblN]]$endif$if[$var[p0]==B]$var[lbl0;$var[lblB]]$endif$if[$var[p0]==R]$var[lbl0;$var[lblR]]$endif$if[$var[p0]==Q]$var[lbl0;$var[lblQ]]$endif$if[$var[p0]==K]$var[lbl0;$var[lblK]]$endif$addStringSelectOption[$var[lbl0] auf $var[from0];$var[from0];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;1;from]!=]$var[p1;$httpResult[moves_by_square;1;piece]]$var[from1;$httpResult[moves_by_square;1;from]]$var[lbl1;$var[lblP]]$if[$var[p1]==N]$var[lbl1;$var[lblN]]$endif$if[$var[p1]==B]$var[lbl1;$var[lblB]]$endif$if[$var[p1]==R]$var[lbl1;$var[lblR]]$endif$if[$var[p1]==Q]$var[lbl1;$var[lblQ]]$endif$if[$var[p1]==K]$var[lbl1;$var[lblK]]$endif$addStringSelectOption[$var[lbl1] auf $var[from1];$var[from1];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;2;from]!=]$var[p2;$httpResult[moves_by_square;2;piece]]$var[from2;$httpResult[moves_by_square;2;from]]$var[lbl2;$var[lblP]]$if[$var[p2]==N]$var[lbl2;$var[lblN]]$endif$if[$var[p2]==B]$var[lbl2;$var[lblB]]$endif$if[$var[p2]==R]$var[lbl2;$var[lblR]]$endif$if[$var[p2]==Q]$var[lbl2;$var[lblQ]]$endif$if[$var[p2]==K]$var[lbl2;$var[lblK]]$endif$addStringSelectOption[$var[lbl2] auf $var[from2];$var[from2];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;3;from]!=]$var[p3;$httpResult[moves_by_square;3;piece]]$var[from3;$httpResult[moves_by_square;3;from]]$var[lbl3;$var[lblP]]$if[$var[p3]==N]$var[lbl3;$var[lblN]]$endif$if[$var[p3]==B]$var[lbl3;$var[lblB]]$endif$if[$var[p3]==R]$var[lbl3;$var[lblR]]$endif$if[$var[p3]==Q]$var[lbl3;$var[lblQ]]$endif$if[$var[p3]==K]$var[lbl3;$var[lblK]]$endif$addStringSelectOption[$var[lbl3] auf $var[from3];$var[from3];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;4;from]!=]$var[p4;$httpResult[moves_by_square;4;piece]]$var[from4;$httpResult[moves_by_square;4;from]]$var[lbl4;$var[lblP]]$if[$var[p4]==N]$var[lbl4;$var[lblN]]$endif$if[$var[p4]==B]$var[lbl4;$var[lblB]]$endif$if[$var[p4]==R]$var[lbl4;$var[lblR]]$endif$if[$var[p4]==Q]$var[lbl4;$var[lblQ]]$endif$if[$var[p4]==K]$var[lbl4;$var[lblK]]$endif$addStringSelectOption[$var[lbl4] auf $var[from4];$var[from4];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;5;from]!=]$var[p5;$httpResult[moves_by_square;5;piece]]$var[from5;$httpResult[moves_by_square;5;from]]$var[lbl5;$var[lblP]]$if[$var[p5]==N]$var[lbl5;$var[lblN]]$endif$if[$var[p5]==B]$var[lbl5;$var[lblB]]$endif$if[$var[p5]==R]$var[lbl5;$var[lblR]]$endif$if[$var[p5]==Q]$var[lbl5;$var[lblQ]]$endif$if[$var[p5]==K]$var[lbl5;$var[lblK]]$endif$addStringSelectOption[$var[lbl5] auf $var[from5];$var[from5];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;6;from]!=]$var[p6;$httpResult[moves_by_square;6;piece]]$var[from6;$httpResult[moves_by_square;6;from]]$var[lbl6;$var[lblP]]$if[$var[p6]==N]$var[lbl6;$var[lblN]]$endif$if[$var[p6]==B]$var[lbl6;$var[lblB]]$endif$if[$var[p6]==R]$var[lbl6;$var[lblR]]$endif$if[$var[p6]==Q]$var[lbl6;$var[lblQ]]$endif$if[$var[p6]==K]$var[lbl6;$var[lblK]]$endif$addStringSelectOption[$var[lbl6] auf $var[from6];$var[from6];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;7;from]!=]$var[p7;$httpResult[moves_by_square;7;piece]]$var[from7;$httpResult[moves_by_square;7;from]]$var[lbl7;$var[lblP]]$if[$var[p7]==N]$var[lbl7;$var[lblN]]$endif$if[$var[p7]==B]$var[lbl7;$var[lblB]]$endif$if[$var[p7]==R]$var[lbl7;$var[lblR]]$endif$if[$var[p7]==Q]$var[lbl7;$var[lblQ]]$endif$if[$var[p7]==K]$var[lbl7;$var[lblK]]$endif$addStringSelectOption[$var[lbl7] auf $var[from7];$var[from7];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;8;from]!=]$var[p8;$httpResult[moves_by_square;8;piece]]$var[from8;$httpResult[moves_by_square;8;from]]$var[lbl8;$var[lblP]]$if[$var[p8]==N]$var[lbl8;$var[lblN]]$endif$if[$var[p8]==B]$var[lbl8;$var[lblB]]$endif$if[$var[p8]==R]$var[lbl8;$var[lblR]]$endif$if[$var[p8]==Q]$var[lbl8;$var[lblQ]]$endif$if[$var[p8]==K]$var[lbl8;$var[lblK]]$endif$addStringSelectOption[$var[lbl8] auf $var[from8];$var[from8];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;9;from]!=]$var[p9;$httpResult[moves_by_square;9;piece]]$var[from9;$httpResult[moves_by_square;9;from]]$var[lbl9;$var[lblP]]$if[$var[p9]==N]$var[lbl9;$var[lblN]]$endif$if[$var[p9]==B]$var[lbl9;$var[lblB]]$endif$if[$var[p9]==R]$var[lbl9;$var[lblR]]$endif$if[$var[p9]==Q]$var[lbl9;$var[lblQ]]$endif$if[$var[p9]==K]$var[lbl9;$var[lblK]]$endif$addStringSelectOption[$var[lbl9] auf $var[from9];$var[from9];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;10;from]!=]$var[p10;$httpResult[moves_by_square;10;piece]]$var[from10;$httpResult[moves_by_square;10;from]]$var[lbl10;$var[lblP]]$if[$var[p10]==N]$var[lbl10;$var[lblN]]$endif$if[$var[p10]==B]$var[lbl10;$var[lblB]]$endif$if[$var[p10]==R]$var[lbl10;$var[lblR]]$endif$if[$var[p10]==Q]$var[lbl10;$var[lblQ]]$endif$if[$var[p10]==K]$var[lbl10;$var[lblK]]$endif$addStringSelectOption[$var[lbl10] auf $var[from10];$var[from10];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;11;from]!=]$var[p11;$httpResult[moves_by_square;11;piece]]$var[from11;$httpResult[moves_by_square;11;from]]$var[lbl11;$var[lblP]]$if[$var[p11]==N]$var[lbl11;$var[lblN]]$endif$if[$var[p11]==B]$var[lbl11;$var[lblB]]$endif$if[$var[p11]==R]$var[lbl11;$var[lblR]]$endif$if[$var[p11]==Q]$var[lbl11;$var[lblQ]]$endif$if[$var[p11]==K]$var[lbl11;$var[lblK]]$endif$addStringSelectOption[$var[lbl11] auf $var[from11];$var[from11];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;12;from]!=]$var[p12;$httpResult[moves_by_square;12;piece]]$var[from12;$httpResult[moves_by_square;12;from]]$var[lbl12;$var[lblP]]$if[$var[p12]==N]$var[lbl12;$var[lblN]]$endif$if[$var[p12]==B]$var[lbl12;$var[lblB]]$endif$if[$var[p12]==R]$var[lbl12;$var[lblR]]$endif$if[$var[p12]==Q]$var[lbl12;$var[lblQ]]$endif$if[$var[p12]==K]$var[lbl12;$var[lblK]]$endif$addStringSelectOption[$var[lbl12] auf $var[from12];$var[from12];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;13;from]!=]$var[p13;$httpResult[moves_by_square;13;piece]]$var[from13;$httpResult[moves_by_square;13;from]]$var[lbl13;$var[lblP]]$if[$var[p13]==N]$var[lbl13;$var[lblN]]$endif$if[$var[p13]==B]$var[lbl13;$var[lblB]]$endif$if[$var[p13]==R]$var[lbl13;$var[lblR]]$endif$if[$var[p13]==Q]$var[lbl13;$var[lblQ]]$endif$if[$var[p13]==K]$var[lbl13;$var[lblK]]$endif$addStringSelectOption[$var[lbl13] auf $var[from13];$var[from13];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;14;from]!=]$var[p14;$httpResult[moves_by_square;14;piece]]$var[from14;$httpResult[moves_by_square;14;from]]$var[lbl14;$var[lblP]]$if[$var[p14]==N]$var[lbl14;$var[lblN]]$endif$if[$var[p14]==B]$var[lbl14;$var[lblB]]$endif$if[$var[p14]==R]$var[lbl14;$var[lblR]]$endif$if[$var[p14]==Q]$var[lbl14;$var[lblQ]]$endif$if[$var[p14]==K]$var[lbl14;$var[lblK]]$endif$addStringSelectOption[$var[lbl14] auf $var[from14];$var[from14];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;15;from]!=]$var[p15;$httpResult[moves_by_square;15;piece]]$var[from15;$httpResult[moves_by_square;15;from]]$var[lbl15;$var[lblP]]$if[$var[p15]==N]$var[lbl15;$var[lblN]]$endif$if[$var[p15]==B]$var[lbl15;$var[lblB]]$endif$if[$var[p15]==R]$var[lbl15;$var[lblR]]$endif$if[$var[p15]==Q]$var[lbl15;$var[lblQ]]$endif$if[$var[p15]==K]$var[lbl15;$var[lblK]]$endif$addStringSelectOption[$var[lbl15] auf $var[from15];$var[from15];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Aufgeben;danger;false;;ctrl]
$addButtonCV2[chdraw~$var[wID]~$var[gID];🤝 Remis bieten;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Spieler:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chforfeit]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Du hast keine aktive Partie. Führe \`$var[trigger]\` aus, um eine zu erstellen.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]
$var[theme;$splitText[8]]
$if[$var[theme]==]
$var[theme;green]
$endif
$var[trigger;$splitText[9]]
$if[$var[trigger]==]
$var[trigger;!chess]
$endif

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Diese Partie ist nicht mehr aktiv.]
$stop
$endif

$if[$var[curStatus]==c]
$ephemeral
$addTextDisplay[ℹ️ Die Herausforderung wurde noch nicht angenommen. Verwende die Schaltfläche 🚪 Abbrechen, um sie abzubrechen.]
$stop
$endif

$if[$var[curStatus]!=p]
$if[$var[curStatus]!=do]
$ephemeral
$addTextDisplay[ℹ️ Diese Partie ist bereits beendet. Verwende 🎲 Neue Partie, um eine andere zu spielen.]
$stop
$endif
$endif

$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ Du bist kein Spieler dieser Partie. Führe \`$var[trigger]\` aus, um deine eigene zu erstellen.]
$stop
$endif
$endif

$defer
$textSplit[$var[fen]; ]
$var[turn;$splitText[2]]
$var[resignerSide;$var[turn]]
$var[winnerSide;b]
$if[$var[resignerSide]==b]$var[winnerSide;w]$endif

$var[finalStatus;$var[winnerSide]]
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|$var[finalStatus]|-|Aufgabe|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[lastMoveSAN;Aufgabe]
$removeAllComponents
$var[cc;#ED4245]
$var[resultMsg;🚪 Partie aufgegeben]
$if[$var[finalStatus]==w]
$var[cc;#57F287]
$var[resultMsg;🏆 Weiß gewinnt! <@$var[wID]>]
$endif
$if[$var[finalStatus]==b]
$var[cc;#57F287]
$var[resultMsg;🏆 Schwarz gewinnt! <@$var[bID]>]
$endif
$if[$var[finalStatus]==d]
$var[cc;#FEE75C]
$var[resultMsg;🤝 Remis! Partie endet unentschieden]
$endif
$var[perspective;white]
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Schach — Partie beendet**

$var[resultMsg]

**Letzter Zug:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Endstellung;false;board]
$addSeparator[true;small;main]
$addTextDisplay[📊 **Ergebnis:** $var[resultMsg];main]
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chnew~$var[wID]~$var[bID];🎲 Neue Partie;success;false;;ctrl]
$addButtonCV2[chclose~$var[wID]~$var[gID];🗑️ Schließen;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Spieler:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chdraw]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Du hast keine aktive Partie. Führe \`$var[trigger]\` aus, um eine zu erstellen.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]
$var[theme;$splitText[8]]
$if[$var[theme]==]
$var[theme;green]
$endif
$var[trigger;$splitText[9]]
$if[$var[trigger]==]
$var[trigger;!chess]
$endif

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Diese Partie ist nicht mehr aktiv.]
$stop
$endif

$if[$var[curStatus]==do]
$textSplit[$var[fen]; ]
$var[turn;$splitText[2]]
$var[offerer;$var[wID]]
$var[responder;$var[bID]]
$if[$var[turn]==b]
$var[offerer;$var[bID]]
$var[responder;$var[wID]]
$endif
$if[$authorID==$var[offerer]]
$ephemeral
$addTextDisplay[ℹ️ Du hast Remis geboten. Warte auf die Antwort deines Gegners.]
$stop
$endif
$if[$authorID!=$var[responder]]
$ephemeral
$addTextDisplay[❌ Nur der Gegner kann auf dieses Angebot antworten.]
$stop
$endif

$defer
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|d|-|Remis vereinbart|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[finalStatus;d]
$var[lastMoveSAN;Remis]
$removeAllComponents
$var[cc;#ED4245]
$var[resultMsg;🚪 Partie aufgegeben]
$if[$var[finalStatus]==w]
$var[cc;#57F287]
$var[resultMsg;🏆 Weiß gewinnt! <@$var[wID]>]
$endif
$if[$var[finalStatus]==b]
$var[cc;#57F287]
$var[resultMsg;🏆 Schwarz gewinnt! <@$var[bID]>]
$endif
$if[$var[finalStatus]==d]
$var[cc;#FEE75C]
$var[resultMsg;🤝 Remis! Partie endet unentschieden]
$endif
$var[perspective;white]
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Schach — Partie beendet**

$var[resultMsg]

**Letzter Zug:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Endstellung;false;board]
$addSeparator[true;small;main]
$addTextDisplay[📊 **Ergebnis:** $var[resultMsg];main]
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chnew~$var[wID]~$var[bID];🎲 Neue Partie;success;false;;ctrl]
$addButtonCV2[chclose~$var[wID]~$var[gID];🗑️ Schließen;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Spieler:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[curStatus]!=p]
$ephemeral
$addTextDisplay[ℹ️ Diese Partie ist bereits beendet. Verwende 🎲 Neue Partie, um eine andere zu spielen.]
$stop
$endif

$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ Du bist kein Spieler dieser Partie. Führe \`$var[trigger]\` aus, um deine eigene zu erstellen.]
$stop
$endif
$endif

$textSplit[$var[fen]; ]
$var[turn;$splitText[2]]
$var[expectedPlayer;$var[wID]]
$var[opponentPlayer;$var[bID]]
$if[$var[turn]==b]
$var[expectedPlayer;$var[bID]]
$var[opponentPlayer;$var[wID]]
$endif
$if[$authorID!=$var[expectedPlayer]]
$ephemeral
$addTextDisplay[❌ Du bist nicht am Zug, um Remis zu bieten.]
$stop
$endif

$defer
$var[perspective;white]

$var[lastMoveSAN2;-]
$textSplit[$var[state];|]
$var[lastMoveSAN2;$splitText[7]]

$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|do|-|$var[lastMoveSAN2]|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$removeAllComponents
$addContainer[main;#FEE75C;false]
$addTextDisplay[# ♟️ **Schach — Remisangebot**

<@$var[expectedPlayer]> bietet <@$var[opponentPlayer]> Remis an.

Nimmst du das Remis an?;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme];Aktuelles Brett;false;board]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chdraw~$var[wID]~$var[gID];✅ Remis annehmen;success;false;;r1]
$addButtonCV2[chdrawdecline~$var[wID]~$var[gID];❌ Remis ablehnen;danger;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**Spieler:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chdrawdecline]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Du hast keine aktive Partie. Führe \`$var[trigger]\` aus, um eine zu erstellen.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]
$var[theme;$splitText[8]]
$if[$var[theme]==]
$var[theme;green]
$endif
$var[trigger;$splitText[9]]
$if[$var[trigger]==]
$var[trigger;!chess]
$endif

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Diese Partie ist nicht mehr aktiv.]
$stop
$endif

$if[$var[curStatus]!=do]
$ephemeral
$addTextDisplay[ℹ️ Es steht kein Remisangebot aus.]
$stop
$endif

$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ Du bist kein Spieler dieser Partie. Führe \`$var[trigger]\` aus, um deine eigene zu erstellen.]
$stop
$endif
$endif

$textSplit[$var[fen]; ]
$var[turn;$splitText[2]]
$var[offerer;$var[wID]]
$var[responder;$var[bID]]
$if[$var[turn]==b]
$var[offerer;$var[bID]]
$var[responder;$var[wID]]
$endif
$if[$authorID!=$var[responder]]
$ephemeral
$addTextDisplay[❌ Nur der Gegner kann auf dieses Angebot antworten.]
$stop
$endif

$defer
$var[lastMoveSAN;-]
$textSplit[$var[state];|]
$var[lastMoveSAN;$splitText[7]]

$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|p|-|$var[lastMoveSAN]|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$removeAllComponents
$httpGet[http://chess.nexusify.co/moves?fen=$url[encode;$var[fen]]]
$var[movesCount;$httpResult[count]]
$var[turn;$httpResult[turn]]
$var[cc;#5865F2]
$var[turnLabel;♔ Weiß]
$var[activePlayer;$var[wID]]
$var[perspective;white]
$if[$var[turn]==b]
$var[turnLabel;♚ Schwarz]
$var[activePlayer;$var[bID]]
$endif
$var[lblP;♙ Bauer]$var[lblN;♘ Springer]$var[lblB;♗ Läufer]$var[lblR;♖ Turm]$var[lblQ;♕ Dame]$var[lblK;♔ König]$if[$var[turn]==b]$var[lblP;♟ Bauer]$var[lblN;♞ Springer]$var[lblB;♝ Läufer]$var[lblR;♜ Turm]$var[lblQ;♛ Dame]$var[lblK;♚ König]$endif
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Schach**

**Zug:** $var[turnLabel]
**Am Zug:** <@$var[activePlayer]>
**Letzter Zug:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Schachbrett;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Dein Zug, <@$var[activePlayer]>.** Wähle eine Figur zum Ziehen:;main]
$addActionRow[r1;main]
$addStringSelect[chpiece~$var[wID]~$var[gID];Figur wählen...;1;1;;r1]
$if[$httpResult[moves_by_square;0;from]!=]$var[p0;$httpResult[moves_by_square;0;piece]]$var[from0;$httpResult[moves_by_square;0;from]]$var[lbl0;$var[lblP]]$if[$var[p0]==N]$var[lbl0;$var[lblN]]$endif$if[$var[p0]==B]$var[lbl0;$var[lblB]]$endif$if[$var[p0]==R]$var[lbl0;$var[lblR]]$endif$if[$var[p0]==Q]$var[lbl0;$var[lblQ]]$endif$if[$var[p0]==K]$var[lbl0;$var[lblK]]$endif$addStringSelectOption[$var[lbl0] auf $var[from0];$var[from0];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;1;from]!=]$var[p1;$httpResult[moves_by_square;1;piece]]$var[from1;$httpResult[moves_by_square;1;from]]$var[lbl1;$var[lblP]]$if[$var[p1]==N]$var[lbl1;$var[lblN]]$endif$if[$var[p1]==B]$var[lbl1;$var[lblB]]$endif$if[$var[p1]==R]$var[lbl1;$var[lblR]]$endif$if[$var[p1]==Q]$var[lbl1;$var[lblQ]]$endif$if[$var[p1]==K]$var[lbl1;$var[lblK]]$endif$addStringSelectOption[$var[lbl1] auf $var[from1];$var[from1];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;2;from]!=]$var[p2;$httpResult[moves_by_square;2;piece]]$var[from2;$httpResult[moves_by_square;2;from]]$var[lbl2;$var[lblP]]$if[$var[p2]==N]$var[lbl2;$var[lblN]]$endif$if[$var[p2]==B]$var[lbl2;$var[lblB]]$endif$if[$var[p2]==R]$var[lbl2;$var[lblR]]$endif$if[$var[p2]==Q]$var[lbl2;$var[lblQ]]$endif$if[$var[p2]==K]$var[lbl2;$var[lblK]]$endif$addStringSelectOption[$var[lbl2] auf $var[from2];$var[from2];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;3;from]!=]$var[p3;$httpResult[moves_by_square;3;piece]]$var[from3;$httpResult[moves_by_square;3;from]]$var[lbl3;$var[lblP]]$if[$var[p3]==N]$var[lbl3;$var[lblN]]$endif$if[$var[p3]==B]$var[lbl3;$var[lblB]]$endif$if[$var[p3]==R]$var[lbl3;$var[lblR]]$endif$if[$var[p3]==Q]$var[lbl3;$var[lblQ]]$endif$if[$var[p3]==K]$var[lbl3;$var[lblK]]$endif$addStringSelectOption[$var[lbl3] auf $var[from3];$var[from3];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;4;from]!=]$var[p4;$httpResult[moves_by_square;4;piece]]$var[from4;$httpResult[moves_by_square;4;from]]$var[lbl4;$var[lblP]]$if[$var[p4]==N]$var[lbl4;$var[lblN]]$endif$if[$var[p4]==B]$var[lbl4;$var[lblB]]$endif$if[$var[p4]==R]$var[lbl4;$var[lblR]]$endif$if[$var[p4]==Q]$var[lbl4;$var[lblQ]]$endif$if[$var[p4]==K]$var[lbl4;$var[lblK]]$endif$addStringSelectOption[$var[lbl4] auf $var[from4];$var[from4];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;5;from]!=]$var[p5;$httpResult[moves_by_square;5;piece]]$var[from5;$httpResult[moves_by_square;5;from]]$var[lbl5;$var[lblP]]$if[$var[p5]==N]$var[lbl5;$var[lblN]]$endif$if[$var[p5]==B]$var[lbl5;$var[lblB]]$endif$if[$var[p5]==R]$var[lbl5;$var[lblR]]$endif$if[$var[p5]==Q]$var[lbl5;$var[lblQ]]$endif$if[$var[p5]==K]$var[lbl5;$var[lblK]]$endif$addStringSelectOption[$var[lbl5] auf $var[from5];$var[from5];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;6;from]!=]$var[p6;$httpResult[moves_by_square;6;piece]]$var[from6;$httpResult[moves_by_square;6;from]]$var[lbl6;$var[lblP]]$if[$var[p6]==N]$var[lbl6;$var[lblN]]$endif$if[$var[p6]==B]$var[lbl6;$var[lblB]]$endif$if[$var[p6]==R]$var[lbl6;$var[lblR]]$endif$if[$var[p6]==Q]$var[lbl6;$var[lblQ]]$endif$if[$var[p6]==K]$var[lbl6;$var[lblK]]$endif$addStringSelectOption[$var[lbl6] auf $var[from6];$var[from6];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;7;from]!=]$var[p7;$httpResult[moves_by_square;7;piece]]$var[from7;$httpResult[moves_by_square;7;from]]$var[lbl7;$var[lblP]]$if[$var[p7]==N]$var[lbl7;$var[lblN]]$endif$if[$var[p7]==B]$var[lbl7;$var[lblB]]$endif$if[$var[p7]==R]$var[lbl7;$var[lblR]]$endif$if[$var[p7]==Q]$var[lbl7;$var[lblQ]]$endif$if[$var[p7]==K]$var[lbl7;$var[lblK]]$endif$addStringSelectOption[$var[lbl7] auf $var[from7];$var[from7];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;8;from]!=]$var[p8;$httpResult[moves_by_square;8;piece]]$var[from8;$httpResult[moves_by_square;8;from]]$var[lbl8;$var[lblP]]$if[$var[p8]==N]$var[lbl8;$var[lblN]]$endif$if[$var[p8]==B]$var[lbl8;$var[lblB]]$endif$if[$var[p8]==R]$var[lbl8;$var[lblR]]$endif$if[$var[p8]==Q]$var[lbl8;$var[lblQ]]$endif$if[$var[p8]==K]$var[lbl8;$var[lblK]]$endif$addStringSelectOption[$var[lbl8] auf $var[from8];$var[from8];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;9;from]!=]$var[p9;$httpResult[moves_by_square;9;piece]]$var[from9;$httpResult[moves_by_square;9;from]]$var[lbl9;$var[lblP]]$if[$var[p9]==N]$var[lbl9;$var[lblN]]$endif$if[$var[p9]==B]$var[lbl9;$var[lblB]]$endif$if[$var[p9]==R]$var[lbl9;$var[lblR]]$endif$if[$var[p9]==Q]$var[lbl9;$var[lblQ]]$endif$if[$var[p9]==K]$var[lbl9;$var[lblK]]$endif$addStringSelectOption[$var[lbl9] auf $var[from9];$var[from9];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;10;from]!=]$var[p10;$httpResult[moves_by_square;10;piece]]$var[from10;$httpResult[moves_by_square;10;from]]$var[lbl10;$var[lblP]]$if[$var[p10]==N]$var[lbl10;$var[lblN]]$endif$if[$var[p10]==B]$var[lbl10;$var[lblB]]$endif$if[$var[p10]==R]$var[lbl10;$var[lblR]]$endif$if[$var[p10]==Q]$var[lbl10;$var[lblQ]]$endif$if[$var[p10]==K]$var[lbl10;$var[lblK]]$endif$addStringSelectOption[$var[lbl10] auf $var[from10];$var[from10];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;11;from]!=]$var[p11;$httpResult[moves_by_square;11;piece]]$var[from11;$httpResult[moves_by_square;11;from]]$var[lbl11;$var[lblP]]$if[$var[p11]==N]$var[lbl11;$var[lblN]]$endif$if[$var[p11]==B]$var[lbl11;$var[lblB]]$endif$if[$var[p11]==R]$var[lbl11;$var[lblR]]$endif$if[$var[p11]==Q]$var[lbl11;$var[lblQ]]$endif$if[$var[p11]==K]$var[lbl11;$var[lblK]]$endif$addStringSelectOption[$var[lbl11] auf $var[from11];$var[from11];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;12;from]!=]$var[p12;$httpResult[moves_by_square;12;piece]]$var[from12;$httpResult[moves_by_square;12;from]]$var[lbl12;$var[lblP]]$if[$var[p12]==N]$var[lbl12;$var[lblN]]$endif$if[$var[p12]==B]$var[lbl12;$var[lblB]]$endif$if[$var[p12]==R]$var[lbl12;$var[lblR]]$endif$if[$var[p12]==Q]$var[lbl12;$var[lblQ]]$endif$if[$var[p12]==K]$var[lbl12;$var[lblK]]$endif$addStringSelectOption[$var[lbl12] auf $var[from12];$var[from12];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;13;from]!=]$var[p13;$httpResult[moves_by_square;13;piece]]$var[from13;$httpResult[moves_by_square;13;from]]$var[lbl13;$var[lblP]]$if[$var[p13]==N]$var[lbl13;$var[lblN]]$endif$if[$var[p13]==B]$var[lbl13;$var[lblB]]$endif$if[$var[p13]==R]$var[lbl13;$var[lblR]]$endif$if[$var[p13]==Q]$var[lbl13;$var[lblQ]]$endif$if[$var[p13]==K]$var[lbl13;$var[lblK]]$endif$addStringSelectOption[$var[lbl13] auf $var[from13];$var[from13];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;14;from]!=]$var[p14;$httpResult[moves_by_square;14;piece]]$var[from14;$httpResult[moves_by_square;14;from]]$var[lbl14;$var[lblP]]$if[$var[p14]==N]$var[lbl14;$var[lblN]]$endif$if[$var[p14]==B]$var[lbl14;$var[lblB]]$endif$if[$var[p14]==R]$var[lbl14;$var[lblR]]$endif$if[$var[p14]==Q]$var[lbl14;$var[lblQ]]$endif$if[$var[p14]==K]$var[lbl14;$var[lblK]]$endif$addStringSelectOption[$var[lbl14] auf $var[from14];$var[from14];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;15;from]!=]$var[p15;$httpResult[moves_by_square;15;piece]]$var[from15;$httpResult[moves_by_square;15;from]]$var[lbl15;$var[lblP]]$if[$var[p15]==N]$var[lbl15;$var[lblN]]$endif$if[$var[p15]==B]$var[lbl15;$var[lblB]]$endif$if[$var[p15]==R]$var[lbl15;$var[lblR]]$endif$if[$var[p15]==Q]$var[lbl15;$var[lblQ]]$endif$if[$var[p15]==K]$var[lbl15;$var[lblK]]$endif$addStringSelectOption[$var[lbl15] auf $var[from15];$var[from15];Figur;;;chpiece~$var[wID]~$var[gID]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Aufgeben;danger;false;;ctrl]
$addButtonCV2[chdraw~$var[wID]~$var[gID];🤝 Remis bieten;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Spieler:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chthemegr]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[newTheme;green]
$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Nur der Herausforderer (<@$var[wID]>) kann das Brett-Thema ändern.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Du hast keine aktive Partie. Führe \`$var[trigger]\` aus, um eine zu erstellen.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Diese Partie ist nicht mehr aktiv. Führe \`$var[trigger]\` aus, um eine neue zu erstellen.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[ℹ️ Du kannst das Thema nur ändern, bevor die Herausforderung angenommen wird.]
$stop
$endif

$defer
$var[theme;$var[newTheme]]
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|$var[curStatus]|-|-|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[challengerID;$var[wID]]
$var[opponentID;$var[bID]]
$var[gameID;$var[gID]]
$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Schach — Herausforderung ausstehend**

<@$var[challengerID]> (♔ Weiß) fordert heraus: <@$var[opponentID]> (♚ Schwarz) zu einer Schachpartie.

⏳ Warte auf Antwort von <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Annehmen;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Ablehnen;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Abbrechen;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**Partie-ID:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Brett wählen** — Nur <@$var[challengerID]> kann das Thema ändern.
Aktuelles Thema: **$var[theme]**;themes]
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
$stop
$endif

$if[$var[action]==chthemebl]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[newTheme;blue]
$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Nur der Herausforderer (<@$var[wID]>) kann das Brett-Thema ändern.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Du hast keine aktive Partie. Führe \`$var[trigger]\` aus, um eine zu erstellen.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Diese Partie ist nicht mehr aktiv. Führe \`$var[trigger]\` aus, um eine neue zu erstellen.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[ℹ️ Du kannst das Thema nur ändern, bevor die Herausforderung angenommen wird.]
$stop
$endif

$defer
$var[theme;$var[newTheme]]
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|$var[curStatus]|-|-|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[challengerID;$var[wID]]
$var[opponentID;$var[bID]]
$var[gameID;$var[gID]]
$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Schach — Herausforderung ausstehend**

<@$var[challengerID]> (♔ Weiß) fordert heraus: <@$var[opponentID]> (♚ Schwarz) zu einer Schachpartie.

⏳ Warte auf Antwort von <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Annehmen;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Ablehnen;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Abbrechen;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**Partie-ID:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Brett wählen** — Nur <@$var[challengerID]> kann das Thema ändern.
Aktuelles Thema: **$var[theme]**;themes]
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
$stop
$endif

$if[$var[action]==chthemebr]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[newTheme;brown]
$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Nur der Herausforderer (<@$var[wID]>) kann das Brett-Thema ändern.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Du hast keine aktive Partie. Führe \`$var[trigger]\` aus, um eine zu erstellen.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Diese Partie ist nicht mehr aktiv. Führe \`$var[trigger]\` aus, um eine neue zu erstellen.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[ℹ️ Du kannst das Thema nur ändern, bevor die Herausforderung angenommen wird.]
$stop
$endif

$defer
$var[theme;$var[newTheme]]
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|$var[curStatus]|-|-|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[challengerID;$var[wID]]
$var[opponentID;$var[bID]]
$var[gameID;$var[gID]]
$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Schach — Herausforderung ausstehend**

<@$var[challengerID]> (♔ Weiß) fordert heraus: <@$var[opponentID]> (♚ Schwarz) zu einer Schachpartie.

⏳ Warte auf Antwort von <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Annehmen;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Ablehnen;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Abbrechen;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**Partie-ID:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Brett wählen** — Nur <@$var[challengerID]> kann das Thema ändern.
Aktuelles Thema: **$var[theme]**;themes]
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
$stop
$endif

$if[$var[action]==chthemepu]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[newTheme;purple]
$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Nur der Herausforderer (<@$var[wID]>) kann das Brett-Thema ändern.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Du hast keine aktive Partie. Führe \`$var[trigger]\` aus, um eine zu erstellen.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Diese Partie ist nicht mehr aktiv. Führe \`$var[trigger]\` aus, um eine neue zu erstellen.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[ℹ️ Du kannst das Thema nur ändern, bevor die Herausforderung angenommen wird.]
$stop
$endif

$defer
$var[theme;$var[newTheme]]
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|$var[curStatus]|-|-|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[challengerID;$var[wID]]
$var[opponentID;$var[bID]]
$var[gameID;$var[gID]]
$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Schach — Herausforderung ausstehend**

<@$var[challengerID]> (♔ Weiß) fordert heraus: <@$var[opponentID]> (♚ Schwarz) zu einer Schachpartie.

⏳ Warte auf Antwort von <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Annehmen;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Ablehnen;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Abbrechen;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**Partie-ID:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Brett wählen** — Nur <@$var[challengerID]> kann das Thema ändern.
Aktuelles Thema: **$var[theme]**;themes]
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
$stop
$endif

# ----- chforce: cancel existing game and create new one -----
$if[$var[action]==chforce]
$var[wID;$splitText[2]]
$var[newOpponentID;$splitText[3]]
$var[oldOpponentID;$splitText[4]]
$var[oldGameID;$splitText[5]]

$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Nur der Herausforderer kann seine vorherige Partie abbrechen.]
$stop
$endif

$defer
$var[oldState;$getVar[chess_state;$var[wID]]]
$var[trigger;!chess]
$if[$var[oldState]!=]
$textSplit[$var[oldState];|]
$var[trigger;$splitText[9]]
$if[$var[trigger]==]
$var[trigger;!chess]
$endif
$endif
$setVar[chess_state;;$var[wID]]
$var[challengerID;$var[wID]]
$var[opponentID;$var[newOpponentID]]
$var[gameID;$randomString[10]]
$var[theme;green]
$var[state;rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1|$var[challengerID]|$var[opponentID]|$var[gameID]|c|-|-|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[state];$var[challengerID]]

$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Schach — Herausforderung ausstehend**

<@$var[challengerID]> (♔ Weiß) fordert heraus: <@$var[opponentID]> (♚ Schwarz) zu einer Schachpartie.

⏳ Warte auf Antwort von <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Annehmen;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Ablehnen;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Abbrechen;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**Partie-ID:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Brett wählen** — Nur <@$var[challengerID]> kann das Thema ändern.
Aktuelles Thema: **$var[theme]**;themes]
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
$stop
$endif

$catch
$ephemeral
$addTextDisplay[❌ Ein unerwarteter Fehler ist aufgetreten: $error[message]]
$endtry
```

# Trigger:

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
$addTextDisplay[❌ Only <@$var[bID]> can accept this challenge.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ This challenge is no longer active. Run \`$var[trigger]\` again.]
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
$addTextDisplay[❌ This challenge is no longer active.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[❌ This challenge has already been answered.]
$stop
$endif

$defer
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|p|-|-|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[lastMoveSAN;-]
$removeAllComponents
$httpGet[http://chess.nexusify.co:26102/moves?fen=$url[encode;$var[fen]]]
$var[movesCount;$httpResult[count]]
$var[turn;$httpResult[turn]]
$var[cc;#5865F2]
$var[turnLabel;♔ White]
$var[activePlayer;$var[wID]]
$var[perspective;white]
$if[$var[turn]==b]
$var[turnLabel;♚ Black]
$var[activePlayer;$var[bID]]
$endif
$var[lblP;♙ Pawn]$var[lblN;♘ Knight]$var[lblB;♗ Bishop]$var[lblR;♖ Rook]$var[lblQ;♕ Queen]$var[lblK;♔ King]$if[$var[turn]==b]$var[lblP;♟ Pawn]$var[lblN;♞ Knight]$var[lblB;♝ Bishop]$var[lblR;♜ Rook]$var[lblQ;♛ Queen]$var[lblK;♚ King]$endif
$var[boardUrl;http://chess.nexusify.co:26102/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Chess**

**Turn:** $var[turnLabel]
**To move:** <@$var[activePlayer]>
**Last move:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Chess board;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Your turn, <@$var[activePlayer]>.** Select a piece to move:;main]
$addActionRow[r1;main]
$addStringSelect[chpiece~$var[wID]~$var[gID];Select piece...;1;1;;r1]
$if[$httpResult[moves_by_square;0;from]!=]$var[p0;$httpResult[moves_by_square;0;piece]]$var[from0;$httpResult[moves_by_square;0;from]]$var[lbl0;$var[lblP]]$if[$var[p0]==N]$var[lbl0;$var[lblN]]$endif$if[$var[p0]==B]$var[lbl0;$var[lblB]]$endif$if[$var[p0]==R]$var[lbl0;$var[lblR]]$endif$if[$var[p0]==Q]$var[lbl0;$var[lblQ]]$endif$if[$var[p0]==K]$var[lbl0;$var[lblK]]$endif$addStringSelectOption[$var[lbl0] at $var[from0];$var[from0];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;1;from]!=]$var[p1;$httpResult[moves_by_square;1;piece]]$var[from1;$httpResult[moves_by_square;1;from]]$var[lbl1;$var[lblP]]$if[$var[p1]==N]$var[lbl1;$var[lblN]]$endif$if[$var[p1]==B]$var[lbl1;$var[lblB]]$endif$if[$var[p1]==R]$var[lbl1;$var[lblR]]$endif$if[$var[p1]==Q]$var[lbl1;$var[lblQ]]$endif$if[$var[p1]==K]$var[lbl1;$var[lblK]]$endif$addStringSelectOption[$var[lbl1] at $var[from1];$var[from1];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;2;from]!=]$var[p2;$httpResult[moves_by_square;2;piece]]$var[from2;$httpResult[moves_by_square;2;from]]$var[lbl2;$var[lblP]]$if[$var[p2]==N]$var[lbl2;$var[lblN]]$endif$if[$var[p2]==B]$var[lbl2;$var[lblB]]$endif$if[$var[p2]==R]$var[lbl2;$var[lblR]]$endif$if[$var[p2]==Q]$var[lbl2;$var[lblQ]]$endif$if[$var[p2]==K]$var[lbl2;$var[lblK]]$endif$addStringSelectOption[$var[lbl2] at $var[from2];$var[from2];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;3;from]!=]$var[p3;$httpResult[moves_by_square;3;piece]]$var[from3;$httpResult[moves_by_square;3;from]]$var[lbl3;$var[lblP]]$if[$var[p3]==N]$var[lbl3;$var[lblN]]$endif$if[$var[p3]==B]$var[lbl3;$var[lblB]]$endif$if[$var[p3]==R]$var[lbl3;$var[lblR]]$endif$if[$var[p3]==Q]$var[lbl3;$var[lblQ]]$endif$if[$var[p3]==K]$var[lbl3;$var[lblK]]$endif$addStringSelectOption[$var[lbl3] at $var[from3];$var[from3];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;4;from]!=]$var[p4;$httpResult[moves_by_square;4;piece]]$var[from4;$httpResult[moves_by_square;4;from]]$var[lbl4;$var[lblP]]$if[$var[p4]==N]$var[lbl4;$var[lblN]]$endif$if[$var[p4]==B]$var[lbl4;$var[lblB]]$endif$if[$var[p4]==R]$var[lbl4;$var[lblR]]$endif$if[$var[p4]==Q]$var[lbl4;$var[lblQ]]$endif$if[$var[p4]==K]$var[lbl4;$var[lblK]]$endif$addStringSelectOption[$var[lbl4] at $var[from4];$var[from4];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;5;from]!=]$var[p5;$httpResult[moves_by_square;5;piece]]$var[from5;$httpResult[moves_by_square;5;from]]$var[lbl5;$var[lblP]]$if[$var[p5]==N]$var[lbl5;$var[lblN]]$endif$if[$var[p5]==B]$var[lbl5;$var[lblB]]$endif$if[$var[p5]==R]$var[lbl5;$var[lblR]]$endif$if[$var[p5]==Q]$var[lbl5;$var[lblQ]]$endif$if[$var[p5]==K]$var[lbl5;$var[lblK]]$endif$addStringSelectOption[$var[lbl5] at $var[from5];$var[from5];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;6;from]!=]$var[p6;$httpResult[moves_by_square;6;piece]]$var[from6;$httpResult[moves_by_square;6;from]]$var[lbl6;$var[lblP]]$if[$var[p6]==N]$var[lbl6;$var[lblN]]$endif$if[$var[p6]==B]$var[lbl6;$var[lblB]]$endif$if[$var[p6]==R]$var[lbl6;$var[lblR]]$endif$if[$var[p6]==Q]$var[lbl6;$var[lblQ]]$endif$if[$var[p6]==K]$var[lbl6;$var[lblK]]$endif$addStringSelectOption[$var[lbl6] at $var[from6];$var[from6];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;7;from]!=]$var[p7;$httpResult[moves_by_square;7;piece]]$var[from7;$httpResult[moves_by_square;7;from]]$var[lbl7;$var[lblP]]$if[$var[p7]==N]$var[lbl7;$var[lblN]]$endif$if[$var[p7]==B]$var[lbl7;$var[lblB]]$endif$if[$var[p7]==R]$var[lbl7;$var[lblR]]$endif$if[$var[p7]==Q]$var[lbl7;$var[lblQ]]$endif$if[$var[p7]==K]$var[lbl7;$var[lblK]]$endif$addStringSelectOption[$var[lbl7] at $var[from7];$var[from7];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;8;from]!=]$var[p8;$httpResult[moves_by_square;8;piece]]$var[from8;$httpResult[moves_by_square;8;from]]$var[lbl8;$var[lblP]]$if[$var[p8]==N]$var[lbl8;$var[lblN]]$endif$if[$var[p8]==B]$var[lbl8;$var[lblB]]$endif$if[$var[p8]==R]$var[lbl8;$var[lblR]]$endif$if[$var[p8]==Q]$var[lbl8;$var[lblQ]]$endif$if[$var[p8]==K]$var[lbl8;$var[lblK]]$endif$addStringSelectOption[$var[lbl8] at $var[from8];$var[from8];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;9;from]!=]$var[p9;$httpResult[moves_by_square;9;piece]]$var[from9;$httpResult[moves_by_square;9;from]]$var[lbl9;$var[lblP]]$if[$var[p9]==N]$var[lbl9;$var[lblN]]$endif$if[$var[p9]==B]$var[lbl9;$var[lblB]]$endif$if[$var[p9]==R]$var[lbl9;$var[lblR]]$endif$if[$var[p9]==Q]$var[lbl9;$var[lblQ]]$endif$if[$var[p9]==K]$var[lbl9;$var[lblK]]$endif$addStringSelectOption[$var[lbl9] at $var[from9];$var[from9];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;10;from]!=]$var[p10;$httpResult[moves_by_square;10;piece]]$var[from10;$httpResult[moves_by_square;10;from]]$var[lbl10;$var[lblP]]$if[$var[p10]==N]$var[lbl10;$var[lblN]]$endif$if[$var[p10]==B]$var[lbl10;$var[lblB]]$endif$if[$var[p10]==R]$var[lbl10;$var[lblR]]$endif$if[$var[p10]==Q]$var[lbl10;$var[lblQ]]$endif$if[$var[p10]==K]$var[lbl10;$var[lblK]]$endif$addStringSelectOption[$var[lbl10] at $var[from10];$var[from10];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;11;from]!=]$var[p11;$httpResult[moves_by_square;11;piece]]$var[from11;$httpResult[moves_by_square;11;from]]$var[lbl11;$var[lblP]]$if[$var[p11]==N]$var[lbl11;$var[lblN]]$endif$if[$var[p11]==B]$var[lbl11;$var[lblB]]$endif$if[$var[p11]==R]$var[lbl11;$var[lblR]]$endif$if[$var[p11]==Q]$var[lbl11;$var[lblQ]]$endif$if[$var[p11]==K]$var[lbl11;$var[lblK]]$endif$addStringSelectOption[$var[lbl11] at $var[from11];$var[from11];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;12;from]!=]$var[p12;$httpResult[moves_by_square;12;piece]]$var[from12;$httpResult[moves_by_square;12;from]]$var[lbl12;$var[lblP]]$if[$var[p12]==N]$var[lbl12;$var[lblN]]$endif$if[$var[p12]==B]$var[lbl12;$var[lblB]]$endif$if[$var[p12]==R]$var[lbl12;$var[lblR]]$endif$if[$var[p12]==Q]$var[lbl12;$var[lblQ]]$endif$if[$var[p12]==K]$var[lbl12;$var[lblK]]$endif$addStringSelectOption[$var[lbl12] at $var[from12];$var[from12];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;13;from]!=]$var[p13;$httpResult[moves_by_square;13;piece]]$var[from13;$httpResult[moves_by_square;13;from]]$var[lbl13;$var[lblP]]$if[$var[p13]==N]$var[lbl13;$var[lblN]]$endif$if[$var[p13]==B]$var[lbl13;$var[lblB]]$endif$if[$var[p13]==R]$var[lbl13;$var[lblR]]$endif$if[$var[p13]==Q]$var[lbl13;$var[lblQ]]$endif$if[$var[p13]==K]$var[lbl13;$var[lblK]]$endif$addStringSelectOption[$var[lbl13] at $var[from13];$var[from13];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;14;from]!=]$var[p14;$httpResult[moves_by_square;14;piece]]$var[from14;$httpResult[moves_by_square;14;from]]$var[lbl14;$var[lblP]]$if[$var[p14]==N]$var[lbl14;$var[lblN]]$endif$if[$var[p14]==B]$var[lbl14;$var[lblB]]$endif$if[$var[p14]==R]$var[lbl14;$var[lblR]]$endif$if[$var[p14]==Q]$var[lbl14;$var[lblQ]]$endif$if[$var[p14]==K]$var[lbl14;$var[lblK]]$endif$addStringSelectOption[$var[lbl14] at $var[from14];$var[from14];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;15;from]!=]$var[p15;$httpResult[moves_by_square;15;piece]]$var[from15;$httpResult[moves_by_square;15;from]]$var[lbl15;$var[lblP]]$if[$var[p15]==N]$var[lbl15;$var[lblN]]$endif$if[$var[p15]==B]$var[lbl15;$var[lblB]]$endif$if[$var[p15]==R]$var[lbl15;$var[lblR]]$endif$if[$var[p15]==Q]$var[lbl15;$var[lblQ]]$endif$if[$var[p15]==K]$var[lbl15;$var[lblK]]$endif$addStringSelectOption[$var[lbl15] at $var[from15];$var[from15];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Resign;danger;false;;ctrl]
$addButtonCV2[chdraw~$var[wID]~$var[gID];🤝 Offer draw;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Players:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chdecline]
$var[wID;$splitText[2]]
$var[bID;$splitText[3]]
$var[gID;$splitText[4]]

$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ Only <@$var[bID]> can decline this challenge.]
$stop
$endif

$defer
$setVar[chess_state;;$var[wID]]
$removeAllComponents
$addContainer[main;#ED4245;false]
$addTextDisplay[# ♟️ **Challenge declined**

<@$var[bID]> declined the challenge from <@$var[wID]>.;main]
$stop
$endif

$if[$var[action]==chcancel]
$var[wID;$splitText[2]]
$var[bID;$splitText[3]]
$var[gID;$splitText[4]]

$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Only <@$var[wID]> can cancel this challenge.]
$stop
$endif

$defer
$setVar[chess_state;;$var[wID]]
$removeAllComponents
$addContainer[main;#ED4245;false]
$addTextDisplay[# ♟️ **Challenge cancelled**

<@$var[wID]> cancelled the challenge.;main]
$stop
$endif

$if[$var[action]==chnew]
$var[oldwID;$splitText[2]]
$var[oldbID;$splitText[3]]

$if[$authorID!=$var[oldwID]]
$if[$authorID!=$var[oldbID]]
$ephemeral
$addTextDisplay[❌ Only the players of this match can start a new one.]
$stop
$endif
$endif

$var[existingState;$getVar[chess_state;$var[oldwID]]]
$if[$var[existingState]!=]
$textSplit[$var[existingState];|]
$var[existingStatus;$splitText[5]]
$if[$var[existingStatus]==p]
$ephemeral
$addTextDisplay[❌ You already have an ongoing game. Cancel the previous game to create a new one.]
$stop
$endif
$if[$var[existingStatus]==c]
$ephemeral
$addTextDisplay[❌ You already have a pending challenge. Cancel the previous challenge to create a new one.]
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
$addTextDisplay[# ♟️ **Chess — New game**

<@$var[oldwID]> (♔ White) challenges <@$var[oldbID]> (♚ Black) to a new chess match.

⏳ Waiting for response from <@$var[oldbID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[oldwID]~$var[oldbID]~$var[gameID];✅ Accept;success;false;;r1]
$addButtonCV2[chdecline~$var[oldwID]~$var[oldbID]~$var[gameID];❌ Decline;danger;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**Game ID:** \`$var[gameID]\`;main]
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
$addTextDisplay[❌ Only the players of this match can close it.]
$stop
$endif

$defer
$setVar[chess_state;;$var[wID]]
$removeAllComponents
$addContainer[main;#2B2D31;false]
$addTextDisplay[# ♟️ **Chess — Closed**

The game was closed.;main]
$stop
$endif

$if[$var[action]==chbp]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ You have no active game. Run \`$var[trigger]\` to create one.]
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
$addTextDisplay[❌ This game is no longer active. Run \`$var[trigger]\` to create a new one.]
$stop
$endif

$if[$var[curStatus]==c]
$ephemeral
$addTextDisplay[ℹ️ The challenge hasn't been accepted yet. Wait for the opponent to accept.]
$stop
$endif

$if[$var[curStatus]==do]
$ephemeral
$addTextDisplay[ℹ️ There's a pending draw offer. Respond to it first.]
$stop
$endif

$if[$var[curStatus]!=p]
$ephemeral
$addTextDisplay[ℹ️ This game has already ended. Use 🎲 New game to play another.]
$stop
$endif

$textSplit[$var[fen]; ]
$var[turn;$splitText[2]]
$var[expectedPlayer;$var[wID]]
$if[$var[turn]==b]$var[expectedPlayer;$var[bID]]$endif
$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ You are not a player in this game.]
$stop
$endif
$endif
$if[$authorID!=$var[expectedPlayer]]
$ephemeral
$addTextDisplay[❌ It's not your turn. Wait for the other player to move.]
$stop
$endif

$defer
$var[lastMoveSAN;-]
$textSplit[$var[state];|]
$var[lastMoveSAN;$splitText[7]]
$removeAllComponents
$httpGet[http://chess.nexusify.co:26102/moves?fen=$url[encode;$var[fen]]]
$var[movesCount;$httpResult[count]]
$var[turn;$httpResult[turn]]
$var[cc;#5865F2]
$var[turnLabel;♔ White]
$var[activePlayer;$var[wID]]
$var[perspective;white]
$if[$var[turn]==b]
$var[turnLabel;♚ Black]
$var[activePlayer;$var[bID]]
$endif
$var[lblP;♙ Pawn]$var[lblN;♘ Knight]$var[lblB;♗ Bishop]$var[lblR;♖ Rook]$var[lblQ;♕ Queen]$var[lblK;♔ King]$if[$var[turn]==b]$var[lblP;♟ Pawn]$var[lblN;♞ Knight]$var[lblB;♝ Bishop]$var[lblR;♜ Rook]$var[lblQ;♛ Queen]$var[lblK;♚ King]$endif
$var[boardUrl;http://chess.nexusify.co:26102/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Chess**

**Turn:** $var[turnLabel]
**To move:** <@$var[activePlayer]>
**Last move:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Chess board;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Your turn, <@$var[activePlayer]>.** Select a piece to move:;main]
$addActionRow[r1;main]
$addStringSelect[chpiece~$var[wID]~$var[gID];Select piece...;1;1;;r1]
$if[$httpResult[moves_by_square;0;from]!=]$var[p0;$httpResult[moves_by_square;0;piece]]$var[from0;$httpResult[moves_by_square;0;from]]$var[lbl0;$var[lblP]]$if[$var[p0]==N]$var[lbl0;$var[lblN]]$endif$if[$var[p0]==B]$var[lbl0;$var[lblB]]$endif$if[$var[p0]==R]$var[lbl0;$var[lblR]]$endif$if[$var[p0]==Q]$var[lbl0;$var[lblQ]]$endif$if[$var[p0]==K]$var[lbl0;$var[lblK]]$endif$addStringSelectOption[$var[lbl0] at $var[from0];$var[from0];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;1;from]!=]$var[p1;$httpResult[moves_by_square;1;piece]]$var[from1;$httpResult[moves_by_square;1;from]]$var[lbl1;$var[lblP]]$if[$var[p1]==N]$var[lbl1;$var[lblN]]$endif$if[$var[p1]==B]$var[lbl1;$var[lblB]]$endif$if[$var[p1]==R]$var[lbl1;$var[lblR]]$endif$if[$var[p1]==Q]$var[lbl1;$var[lblQ]]$endif$if[$var[p1]==K]$var[lbl1;$var[lblK]]$endif$addStringSelectOption[$var[lbl1] at $var[from1];$var[from1];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;2;from]!=]$var[p2;$httpResult[moves_by_square;2;piece]]$var[from2;$httpResult[moves_by_square;2;from]]$var[lbl2;$var[lblP]]$if[$var[p2]==N]$var[lbl2;$var[lblN]]$endif$if[$var[p2]==B]$var[lbl2;$var[lblB]]$endif$if[$var[p2]==R]$var[lbl2;$var[lblR]]$endif$if[$var[p2]==Q]$var[lbl2;$var[lblQ]]$endif$if[$var[p2]==K]$var[lbl2;$var[lblK]]$endif$addStringSelectOption[$var[lbl2] at $var[from2];$var[from2];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;3;from]!=]$var[p3;$httpResult[moves_by_square;3;piece]]$var[from3;$httpResult[moves_by_square;3;from]]$var[lbl3;$var[lblP]]$if[$var[p3]==N]$var[lbl3;$var[lblN]]$endif$if[$var[p3]==B]$var[lbl3;$var[lblB]]$endif$if[$var[p3]==R]$var[lbl3;$var[lblR]]$endif$if[$var[p3]==Q]$var[lbl3;$var[lblQ]]$endif$if[$var[p3]==K]$var[lbl3;$var[lblK]]$endif$addStringSelectOption[$var[lbl3] at $var[from3];$var[from3];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;4;from]!=]$var[p4;$httpResult[moves_by_square;4;piece]]$var[from4;$httpResult[moves_by_square;4;from]]$var[lbl4;$var[lblP]]$if[$var[p4]==N]$var[lbl4;$var[lblN]]$endif$if[$var[p4]==B]$var[lbl4;$var[lblB]]$endif$if[$var[p4]==R]$var[lbl4;$var[lblR]]$endif$if[$var[p4]==Q]$var[lbl4;$var[lblQ]]$endif$if[$var[p4]==K]$var[lbl4;$var[lblK]]$endif$addStringSelectOption[$var[lbl4] at $var[from4];$var[from4];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;5;from]!=]$var[p5;$httpResult[moves_by_square;5;piece]]$var[from5;$httpResult[moves_by_square;5;from]]$var[lbl5;$var[lblP]]$if[$var[p5]==N]$var[lbl5;$var[lblN]]$endif$if[$var[p5]==B]$var[lbl5;$var[lblB]]$endif$if[$var[p5]==R]$var[lbl5;$var[lblR]]$endif$if[$var[p5]==Q]$var[lbl5;$var[lblQ]]$endif$if[$var[p5]==K]$var[lbl5;$var[lblK]]$endif$addStringSelectOption[$var[lbl5] at $var[from5];$var[from5];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;6;from]!=]$var[p6;$httpResult[moves_by_square;6;piece]]$var[from6;$httpResult[moves_by_square;6;from]]$var[lbl6;$var[lblP]]$if[$var[p6]==N]$var[lbl6;$var[lblN]]$endif$if[$var[p6]==B]$var[lbl6;$var[lblB]]$endif$if[$var[p6]==R]$var[lbl6;$var[lblR]]$endif$if[$var[p6]==Q]$var[lbl6;$var[lblQ]]$endif$if[$var[p6]==K]$var[lbl6;$var[lblK]]$endif$addStringSelectOption[$var[lbl6] at $var[from6];$var[from6];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;7;from]!=]$var[p7;$httpResult[moves_by_square;7;piece]]$var[from7;$httpResult[moves_by_square;7;from]]$var[lbl7;$var[lblP]]$if[$var[p7]==N]$var[lbl7;$var[lblN]]$endif$if[$var[p7]==B]$var[lbl7;$var[lblB]]$endif$if[$var[p7]==R]$var[lbl7;$var[lblR]]$endif$if[$var[p7]==Q]$var[lbl7;$var[lblQ]]$endif$if[$var[p7]==K]$var[lbl7;$var[lblK]]$endif$addStringSelectOption[$var[lbl7] at $var[from7];$var[from7];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;8;from]!=]$var[p8;$httpResult[moves_by_square;8;piece]]$var[from8;$httpResult[moves_by_square;8;from]]$var[lbl8;$var[lblP]]$if[$var[p8]==N]$var[lbl8;$var[lblN]]$endif$if[$var[p8]==B]$var[lbl8;$var[lblB]]$endif$if[$var[p8]==R]$var[lbl8;$var[lblR]]$endif$if[$var[p8]==Q]$var[lbl8;$var[lblQ]]$endif$if[$var[p8]==K]$var[lbl8;$var[lblK]]$endif$addStringSelectOption[$var[lbl8] at $var[from8];$var[from8];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;9;from]!=]$var[p9;$httpResult[moves_by_square;9;piece]]$var[from9;$httpResult[moves_by_square;9;from]]$var[lbl9;$var[lblP]]$if[$var[p9]==N]$var[lbl9;$var[lblN]]$endif$if[$var[p9]==B]$var[lbl9;$var[lblB]]$endif$if[$var[p9]==R]$var[lbl9;$var[lblR]]$endif$if[$var[p9]==Q]$var[lbl9;$var[lblQ]]$endif$if[$var[p9]==K]$var[lbl9;$var[lblK]]$endif$addStringSelectOption[$var[lbl9] at $var[from9];$var[from9];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;10;from]!=]$var[p10;$httpResult[moves_by_square;10;piece]]$var[from10;$httpResult[moves_by_square;10;from]]$var[lbl10;$var[lblP]]$if[$var[p10]==N]$var[lbl10;$var[lblN]]$endif$if[$var[p10]==B]$var[lbl10;$var[lblB]]$endif$if[$var[p10]==R]$var[lbl10;$var[lblR]]$endif$if[$var[p10]==Q]$var[lbl10;$var[lblQ]]$endif$if[$var[p10]==K]$var[lbl10;$var[lblK]]$endif$addStringSelectOption[$var[lbl10] at $var[from10];$var[from10];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;11;from]!=]$var[p11;$httpResult[moves_by_square;11;piece]]$var[from11;$httpResult[moves_by_square;11;from]]$var[lbl11;$var[lblP]]$if[$var[p11]==N]$var[lbl11;$var[lblN]]$endif$if[$var[p11]==B]$var[lbl11;$var[lblB]]$endif$if[$var[p11]==R]$var[lbl11;$var[lblR]]$endif$if[$var[p11]==Q]$var[lbl11;$var[lblQ]]$endif$if[$var[p11]==K]$var[lbl11;$var[lblK]]$endif$addStringSelectOption[$var[lbl11] at $var[from11];$var[from11];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;12;from]!=]$var[p12;$httpResult[moves_by_square;12;piece]]$var[from12;$httpResult[moves_by_square;12;from]]$var[lbl12;$var[lblP]]$if[$var[p12]==N]$var[lbl12;$var[lblN]]$endif$if[$var[p12]==B]$var[lbl12;$var[lblB]]$endif$if[$var[p12]==R]$var[lbl12;$var[lblR]]$endif$if[$var[p12]==Q]$var[lbl12;$var[lblQ]]$endif$if[$var[p12]==K]$var[lbl12;$var[lblK]]$endif$addStringSelectOption[$var[lbl12] at $var[from12];$var[from12];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;13;from]!=]$var[p13;$httpResult[moves_by_square;13;piece]]$var[from13;$httpResult[moves_by_square;13;from]]$var[lbl13;$var[lblP]]$if[$var[p13]==N]$var[lbl13;$var[lblN]]$endif$if[$var[p13]==B]$var[lbl13;$var[lblB]]$endif$if[$var[p13]==R]$var[lbl13;$var[lblR]]$endif$if[$var[p13]==Q]$var[lbl13;$var[lblQ]]$endif$if[$var[p13]==K]$var[lbl13;$var[lblK]]$endif$addStringSelectOption[$var[lbl13] at $var[from13];$var[from13];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;14;from]!=]$var[p14;$httpResult[moves_by_square;14;piece]]$var[from14;$httpResult[moves_by_square;14;from]]$var[lbl14;$var[lblP]]$if[$var[p14]==N]$var[lbl14;$var[lblN]]$endif$if[$var[p14]==B]$var[lbl14;$var[lblB]]$endif$if[$var[p14]==R]$var[lbl14;$var[lblR]]$endif$if[$var[p14]==Q]$var[lbl14;$var[lblQ]]$endif$if[$var[p14]==K]$var[lbl14;$var[lblK]]$endif$addStringSelectOption[$var[lbl14] at $var[from14];$var[from14];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;15;from]!=]$var[p15;$httpResult[moves_by_square;15;piece]]$var[from15;$httpResult[moves_by_square;15;from]]$var[lbl15;$var[lblP]]$if[$var[p15]==N]$var[lbl15;$var[lblN]]$endif$if[$var[p15]==B]$var[lbl15;$var[lblB]]$endif$if[$var[p15]==R]$var[lbl15;$var[lblR]]$endif$if[$var[p15]==Q]$var[lbl15;$var[lblQ]]$endif$if[$var[p15]==K]$var[lbl15;$var[lblK]]$endif$addStringSelectOption[$var[lbl15] at $var[from15];$var[from15];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Resign;danger;false;;ctrl]
$addButtonCV2[chdraw~$var[wID]~$var[gID];🤝 Offer draw;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Players:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chforfeit]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ You have no active game. Run \`$var[trigger]\` to create one.]
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
$addTextDisplay[❌ This game is no longer active.]
$stop
$endif

$if[$var[curStatus]==c]
$ephemeral
$addTextDisplay[ℹ️ The challenge hasn't been accepted yet. Use the 🚪 Cancel button to cancel it.]
$stop
$endif

$if[$var[curStatus]!=p]
$if[$var[curStatus]!=do]
$ephemeral
$addTextDisplay[ℹ️ This game has already ended. Use 🎲 New game to play another.]
$stop
$endif
$endif

$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ You are not a player in this game. Run \`$var[trigger]\` to create your own.]
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
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|$var[finalStatus]|-|Resignation|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[lastMoveSAN;Resignation]
$removeAllComponents
$var[cc;#ED4245]
$var[resultMsg;🚪 Game abandoned]
$if[$var[finalStatus]==w]
$var[cc;#57F287]
$var[resultMsg;🏆 White wins! <@$var[wID]>]
$endif
$if[$var[finalStatus]==b]
$var[cc;#57F287]
$var[resultMsg;🏆 Black wins! <@$var[bID]>]
$endif
$if[$var[finalStatus]==d]
$var[cc;#FEE75C]
$var[resultMsg;🤝 Draw! Game ended in a tie]
$endif
$var[perspective;white]
$var[boardUrl;http://chess.nexusify.co:26102/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Chess — Game over**

$var[resultMsg]

**Last move:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Final board;false;board]
$addSeparator[true;small;main]
$addTextDisplay[📊 **Result:** $var[resultMsg];main]
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chnew~$var[wID]~$var[bID];🎲 New game;success;false;;ctrl]
$addButtonCV2[chclose~$var[wID]~$var[gID];🗑️ Close;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Players:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chdraw]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ You have no active game. Run \`$var[trigger]\` to create one.]
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
$addTextDisplay[❌ This game is no longer active.]
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
$addTextDisplay[ℹ️ You offered a draw. Wait for your opponent's response.]
$stop
$endif
$if[$authorID!=$var[responder]]
$ephemeral
$addTextDisplay[❌ Only the opponent can respond to this offer.]
$stop
$endif

$defer
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|d|-|Draw agreed|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[finalStatus;d]
$var[lastMoveSAN;Draw]
$removeAllComponents
$var[cc;#ED4245]
$var[resultMsg;🚪 Game abandoned]
$if[$var[finalStatus]==w]
$var[cc;#57F287]
$var[resultMsg;🏆 White wins! <@$var[wID]>]
$endif
$if[$var[finalStatus]==b]
$var[cc;#57F287]
$var[resultMsg;🏆 Black wins! <@$var[bID]>]
$endif
$if[$var[finalStatus]==d]
$var[cc;#FEE75C]
$var[resultMsg;🤝 Draw! Game ended in a tie]
$endif
$var[perspective;white]
$var[boardUrl;http://chess.nexusify.co:26102/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Chess — Game over**

$var[resultMsg]

**Last move:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Final board;false;board]
$addSeparator[true;small;main]
$addTextDisplay[📊 **Result:** $var[resultMsg];main]
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chnew~$var[wID]~$var[bID];🎲 New game;success;false;;ctrl]
$addButtonCV2[chclose~$var[wID]~$var[gID];🗑️ Close;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Players:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[curStatus]!=p]
$ephemeral
$addTextDisplay[ℹ️ This game has already ended. Use 🎲 New game to play another.]
$stop
$endif

$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ You are not a player in this game. Run \`$var[trigger]\` to create your own.]
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
$addTextDisplay[❌ It's not your turn to offer a draw.]
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
$addTextDisplay[# ♟️ **Chess — Draw offer**

<@$var[expectedPlayer]> offers a draw to <@$var[opponentPlayer]>.

Do you accept the draw?;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[http://chess.nexusify.co:26102/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme];Current board;false;board]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chdraw~$var[wID]~$var[gID];✅ Accept draw;success;false;;r1]
$addButtonCV2[chdrawdecline~$var[wID]~$var[gID];❌ Decline draw;danger;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**Players:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chdrawdecline]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ You have no active game. Run \`$var[trigger]\` to create one.]
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
$addTextDisplay[❌ This game is no longer active.]
$stop
$endif

$if[$var[curStatus]!=do]
$ephemeral
$addTextDisplay[ℹ️ No pending draw offer.]
$stop
$endif

$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ You are not a player in this game. Run \`$var[trigger]\` to create your own.]
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
$addTextDisplay[❌ Only the opponent can respond to this offer.]
$stop
$endif

$defer
$var[lastMoveSAN;-]
$textSplit[$var[state];|]
$var[lastMoveSAN;$splitText[7]]

$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|p|-|$var[lastMoveSAN]|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$removeAllComponents
$httpGet[http://chess.nexusify.co:26102/moves?fen=$url[encode;$var[fen]]]
$var[movesCount;$httpResult[count]]
$var[turn;$httpResult[turn]]
$var[cc;#5865F2]
$var[turnLabel;♔ White]
$var[activePlayer;$var[wID]]
$var[perspective;white]
$if[$var[turn]==b]
$var[turnLabel;♚ Black]
$var[activePlayer;$var[bID]]
$endif
$var[lblP;♙ Pawn]$var[lblN;♘ Knight]$var[lblB;♗ Bishop]$var[lblR;♖ Rook]$var[lblQ;♕ Queen]$var[lblK;♔ King]$if[$var[turn]==b]$var[lblP;♟ Pawn]$var[lblN;♞ Knight]$var[lblB;♝ Bishop]$var[lblR;♜ Rook]$var[lblQ;♛ Queen]$var[lblK;♚ King]$endif
$var[boardUrl;http://chess.nexusify.co:26102/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Chess**

**Turn:** $var[turnLabel]
**To move:** <@$var[activePlayer]>
**Last move:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Chess board;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Your turn, <@$var[activePlayer]>.** Select a piece to move:;main]
$addActionRow[r1;main]
$addStringSelect[chpiece~$var[wID]~$var[gID];Select piece...;1;1;;r1]
$if[$httpResult[moves_by_square;0;from]!=]$var[p0;$httpResult[moves_by_square;0;piece]]$var[from0;$httpResult[moves_by_square;0;from]]$var[lbl0;$var[lblP]]$if[$var[p0]==N]$var[lbl0;$var[lblN]]$endif$if[$var[p0]==B]$var[lbl0;$var[lblB]]$endif$if[$var[p0]==R]$var[lbl0;$var[lblR]]$endif$if[$var[p0]==Q]$var[lbl0;$var[lblQ]]$endif$if[$var[p0]==K]$var[lbl0;$var[lblK]]$endif$addStringSelectOption[$var[lbl0] at $var[from0];$var[from0];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;1;from]!=]$var[p1;$httpResult[moves_by_square;1;piece]]$var[from1;$httpResult[moves_by_square;1;from]]$var[lbl1;$var[lblP]]$if[$var[p1]==N]$var[lbl1;$var[lblN]]$endif$if[$var[p1]==B]$var[lbl1;$var[lblB]]$endif$if[$var[p1]==R]$var[lbl1;$var[lblR]]$endif$if[$var[p1]==Q]$var[lbl1;$var[lblQ]]$endif$if[$var[p1]==K]$var[lbl1;$var[lblK]]$endif$addStringSelectOption[$var[lbl1] at $var[from1];$var[from1];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;2;from]!=]$var[p2;$httpResult[moves_by_square;2;piece]]$var[from2;$httpResult[moves_by_square;2;from]]$var[lbl2;$var[lblP]]$if[$var[p2]==N]$var[lbl2;$var[lblN]]$endif$if[$var[p2]==B]$var[lbl2;$var[lblB]]$endif$if[$var[p2]==R]$var[lbl2;$var[lblR]]$endif$if[$var[p2]==Q]$var[lbl2;$var[lblQ]]$endif$if[$var[p2]==K]$var[lbl2;$var[lblK]]$endif$addStringSelectOption[$var[lbl2] at $var[from2];$var[from2];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;3;from]!=]$var[p3;$httpResult[moves_by_square;3;piece]]$var[from3;$httpResult[moves_by_square;3;from]]$var[lbl3;$var[lblP]]$if[$var[p3]==N]$var[lbl3;$var[lblN]]$endif$if[$var[p3]==B]$var[lbl3;$var[lblB]]$endif$if[$var[p3]==R]$var[lbl3;$var[lblR]]$endif$if[$var[p3]==Q]$var[lbl3;$var[lblQ]]$endif$if[$var[p3]==K]$var[lbl3;$var[lblK]]$endif$addStringSelectOption[$var[lbl3] at $var[from3];$var[from3];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;4;from]!=]$var[p4;$httpResult[moves_by_square;4;piece]]$var[from4;$httpResult[moves_by_square;4;from]]$var[lbl4;$var[lblP]]$if[$var[p4]==N]$var[lbl4;$var[lblN]]$endif$if[$var[p4]==B]$var[lbl4;$var[lblB]]$endif$if[$var[p4]==R]$var[lbl4;$var[lblR]]$endif$if[$var[p4]==Q]$var[lbl4;$var[lblQ]]$endif$if[$var[p4]==K]$var[lbl4;$var[lblK]]$endif$addStringSelectOption[$var[lbl4] at $var[from4];$var[from4];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;5;from]!=]$var[p5;$httpResult[moves_by_square;5;piece]]$var[from5;$httpResult[moves_by_square;5;from]]$var[lbl5;$var[lblP]]$if[$var[p5]==N]$var[lbl5;$var[lblN]]$endif$if[$var[p5]==B]$var[lbl5;$var[lblB]]$endif$if[$var[p5]==R]$var[lbl5;$var[lblR]]$endif$if[$var[p5]==Q]$var[lbl5;$var[lblQ]]$endif$if[$var[p5]==K]$var[lbl5;$var[lblK]]$endif$addStringSelectOption[$var[lbl5] at $var[from5];$var[from5];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;6;from]!=]$var[p6;$httpResult[moves_by_square;6;piece]]$var[from6;$httpResult[moves_by_square;6;from]]$var[lbl6;$var[lblP]]$if[$var[p6]==N]$var[lbl6;$var[lblN]]$endif$if[$var[p6]==B]$var[lbl6;$var[lblB]]$endif$if[$var[p6]==R]$var[lbl6;$var[lblR]]$endif$if[$var[p6]==Q]$var[lbl6;$var[lblQ]]$endif$if[$var[p6]==K]$var[lbl6;$var[lblK]]$endif$addStringSelectOption[$var[lbl6] at $var[from6];$var[from6];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;7;from]!=]$var[p7;$httpResult[moves_by_square;7;piece]]$var[from7;$httpResult[moves_by_square;7;from]]$var[lbl7;$var[lblP]]$if[$var[p7]==N]$var[lbl7;$var[lblN]]$endif$if[$var[p7]==B]$var[lbl7;$var[lblB]]$endif$if[$var[p7]==R]$var[lbl7;$var[lblR]]$endif$if[$var[p7]==Q]$var[lbl7;$var[lblQ]]$endif$if[$var[p7]==K]$var[lbl7;$var[lblK]]$endif$addStringSelectOption[$var[lbl7] at $var[from7];$var[from7];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;8;from]!=]$var[p8;$httpResult[moves_by_square;8;piece]]$var[from8;$httpResult[moves_by_square;8;from]]$var[lbl8;$var[lblP]]$if[$var[p8]==N]$var[lbl8;$var[lblN]]$endif$if[$var[p8]==B]$var[lbl8;$var[lblB]]$endif$if[$var[p8]==R]$var[lbl8;$var[lblR]]$endif$if[$var[p8]==Q]$var[lbl8;$var[lblQ]]$endif$if[$var[p8]==K]$var[lbl8;$var[lblK]]$endif$addStringSelectOption[$var[lbl8] at $var[from8];$var[from8];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;9;from]!=]$var[p9;$httpResult[moves_by_square;9;piece]]$var[from9;$httpResult[moves_by_square;9;from]]$var[lbl9;$var[lblP]]$if[$var[p9]==N]$var[lbl9;$var[lblN]]$endif$if[$var[p9]==B]$var[lbl9;$var[lblB]]$endif$if[$var[p9]==R]$var[lbl9;$var[lblR]]$endif$if[$var[p9]==Q]$var[lbl9;$var[lblQ]]$endif$if[$var[p9]==K]$var[lbl9;$var[lblK]]$endif$addStringSelectOption[$var[lbl9] at $var[from9];$var[from9];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;10;from]!=]$var[p10;$httpResult[moves_by_square;10;piece]]$var[from10;$httpResult[moves_by_square;10;from]]$var[lbl10;$var[lblP]]$if[$var[p10]==N]$var[lbl10;$var[lblN]]$endif$if[$var[p10]==B]$var[lbl10;$var[lblB]]$endif$if[$var[p10]==R]$var[lbl10;$var[lblR]]$endif$if[$var[p10]==Q]$var[lbl10;$var[lblQ]]$endif$if[$var[p10]==K]$var[lbl10;$var[lblK]]$endif$addStringSelectOption[$var[lbl10] at $var[from10];$var[from10];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;11;from]!=]$var[p11;$httpResult[moves_by_square;11;piece]]$var[from11;$httpResult[moves_by_square;11;from]]$var[lbl11;$var[lblP]]$if[$var[p11]==N]$var[lbl11;$var[lblN]]$endif$if[$var[p11]==B]$var[lbl11;$var[lblB]]$endif$if[$var[p11]==R]$var[lbl11;$var[lblR]]$endif$if[$var[p11]==Q]$var[lbl11;$var[lblQ]]$endif$if[$var[p11]==K]$var[lbl11;$var[lblK]]$endif$addStringSelectOption[$var[lbl11] at $var[from11];$var[from11];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;12;from]!=]$var[p12;$httpResult[moves_by_square;12;piece]]$var[from12;$httpResult[moves_by_square;12;from]]$var[lbl12;$var[lblP]]$if[$var[p12]==N]$var[lbl12;$var[lblN]]$endif$if[$var[p12]==B]$var[lbl12;$var[lblB]]$endif$if[$var[p12]==R]$var[lbl12;$var[lblR]]$endif$if[$var[p12]==Q]$var[lbl12;$var[lblQ]]$endif$if[$var[p12]==K]$var[lbl12;$var[lblK]]$endif$addStringSelectOption[$var[lbl12] at $var[from12];$var[from12];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;13;from]!=]$var[p13;$httpResult[moves_by_square;13;piece]]$var[from13;$httpResult[moves_by_square;13;from]]$var[lbl13;$var[lblP]]$if[$var[p13]==N]$var[lbl13;$var[lblN]]$endif$if[$var[p13]==B]$var[lbl13;$var[lblB]]$endif$if[$var[p13]==R]$var[lbl13;$var[lblR]]$endif$if[$var[p13]==Q]$var[lbl13;$var[lblQ]]$endif$if[$var[p13]==K]$var[lbl13;$var[lblK]]$endif$addStringSelectOption[$var[lbl13] at $var[from13];$var[from13];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;14;from]!=]$var[p14;$httpResult[moves_by_square;14;piece]]$var[from14;$httpResult[moves_by_square;14;from]]$var[lbl14;$var[lblP]]$if[$var[p14]==N]$var[lbl14;$var[lblN]]$endif$if[$var[p14]==B]$var[lbl14;$var[lblB]]$endif$if[$var[p14]==R]$var[lbl14;$var[lblR]]$endif$if[$var[p14]==Q]$var[lbl14;$var[lblQ]]$endif$if[$var[p14]==K]$var[lbl14;$var[lblK]]$endif$addStringSelectOption[$var[lbl14] at $var[from14];$var[from14];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;15;from]!=]$var[p15;$httpResult[moves_by_square;15;piece]]$var[from15;$httpResult[moves_by_square;15;from]]$var[lbl15;$var[lblP]]$if[$var[p15]==N]$var[lbl15;$var[lblN]]$endif$if[$var[p15]==B]$var[lbl15;$var[lblB]]$endif$if[$var[p15]==R]$var[lbl15;$var[lblR]]$endif$if[$var[p15]==Q]$var[lbl15;$var[lblQ]]$endif$if[$var[p15]==K]$var[lbl15;$var[lblK]]$endif$addStringSelectOption[$var[lbl15] at $var[from15];$var[from15];Piece;;;chpiece~$var[wID]~$var[gID]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Resign;danger;false;;ctrl]
$addButtonCV2[chdraw~$var[wID]~$var[gID];🤝 Offer draw;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Players:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chthemegr]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[newTheme;green]
$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Only the challenger (<@$var[wID]>) can change the board theme.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ You have no active game. Run \`$var[trigger]\` to create one.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ This game is no longer active. Run \`$var[trigger]\` to create a new one.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[ℹ️ You can only change the theme before the challenge is accepted.]
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
$stop
$endif

$if[$var[action]==chthemebl]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[newTheme;blue]
$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Only the challenger (<@$var[wID]>) can change the board theme.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ You have no active game. Run \`$var[trigger]\` to create one.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ This game is no longer active. Run \`$var[trigger]\` to create a new one.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[ℹ️ You can only change the theme before the challenge is accepted.]
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
$stop
$endif

$if[$var[action]==chthemebr]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[newTheme;brown]
$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Only the challenger (<@$var[wID]>) can change the board theme.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ You have no active game. Run \`$var[trigger]\` to create one.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ This game is no longer active. Run \`$var[trigger]\` to create a new one.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[ℹ️ You can only change the theme before the challenge is accepted.]
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
$stop
$endif

$if[$var[action]==chthemepu]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[newTheme;purple]
$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Only the challenger (<@$var[wID]>) can change the board theme.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ You have no active game. Run \`$var[trigger]\` to create one.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ This game is no longer active. Run \`$var[trigger]\` to create a new one.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[ℹ️ You can only change the theme before the challenge is accepted.]
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
$addTextDisplay[❌ Only the challenger can cancel their previous game.]
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
$stop
$endif

$catch
$ephemeral
$addTextDisplay[❌ An unexpected error occurred: $error[message]]
$endtry
```

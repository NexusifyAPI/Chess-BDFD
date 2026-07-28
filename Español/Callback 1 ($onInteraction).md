# Disparador:

```
$onInteraction
```

# Código:

```
$nomention
$disableInnerSpaceRemoval

$if[$checkContains[$customID;chaccept~;chdecline~;chcancel~;chnew~;chclose~;chbp~;chforfeit~;chdraw~;chdrawdecline~;chthemegr~;chthemebl~;chthemebr~;chthemepu~]==false]
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
$addTextDisplay[❌ Solo <@$var[bID]> puede aceptar este reto.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Este reto ya no está activo. Ejecuta \`$commandTrigger\` de nuevo.]
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
$addTextDisplay[❌ Este reto ya no está activo.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[❌ Este reto ya fue respondido.]
$stop
$endif

$defer
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|p|-|-|$var[theme]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[lastMoveSAN;-]
$removeAllComponents
$httpGet[http://chess.nexusify.co/moves?fen=$url[encode;$var[fen]]]
$var[movesCount;$httpResult[count]]
$var[turn;$httpResult[turn]]
$var[cc;#5865F2]
$var[turnLabel;♔ Blancas]
$var[activePlayer;$var[wID]]
$var[perspective;white]
$if[$var[turn]==b]
$var[turnLabel;♚ Negras]
$var[activePlayer;$var[bID]]
$endif
$var[lblP;♙ Peón]$var[lblN;♘ Caballo]$var[lblB;♗ Alfil]$var[lblR;♖ Torre]$var[lblQ;♕ Dama]$var[lblK;♔ Rey]$if[$var[turn]==b]$var[lblP;♟ Peón]$var[lblN;♞ Caballo]$var[lblB;♝ Alfil]$var[lblR;♜ Torre]$var[lblQ;♛ Dama]$var[lblK;♚ Rey]$endif
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Ajedrez**

**Turno:** $var[turnLabel]
**Mueve:** <@$var[activePlayer]>
**Última jugada:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Tablero de ajedrez;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Tu turno, <@$var[activePlayer]>.** Selecciona una pieza para mover:;main]
$addActionRow[r1;main]
$addStringSelect[chpiece~$var[wID]~$var[gID];Selecciona pieza...;1;1;;r1]
$if[$httpResult[moves_by_square;0;from]!=]$var[p0;$httpResult[moves_by_square;0;piece]]$var[from0;$httpResult[moves_by_square;0;from]]$var[lbl0;$var[lblP]]$if[$var[p0]==N]$var[lbl0;$var[lblN]]$endif$if[$var[p0]==B]$var[lbl0;$var[lblB]]$endif$if[$var[p0]==R]$var[lbl0;$var[lblR]]$endif$if[$var[p0]==Q]$var[lbl0;$var[lblQ]]$endif$if[$var[p0]==K]$var[lbl0;$var[lblK]]$endif$addStringSelectOption[$var[lbl0] en $var[from0];$var[from0];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;1;from]!=]$var[p1;$httpResult[moves_by_square;1;piece]]$var[from1;$httpResult[moves_by_square;1;from]]$var[lbl1;$var[lblP]]$if[$var[p1]==N]$var[lbl1;$var[lblN]]$endif$if[$var[p1]==B]$var[lbl1;$var[lblB]]$endif$if[$var[p1]==R]$var[lbl1;$var[lblR]]$endif$if[$var[p1]==Q]$var[lbl1;$var[lblQ]]$endif$if[$var[p1]==K]$var[lbl1;$var[lblK]]$endif$addStringSelectOption[$var[lbl1] en $var[from1];$var[from1];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;2;from]!=]$var[p2;$httpResult[moves_by_square;2;piece]]$var[from2;$httpResult[moves_by_square;2;from]]$var[lbl2;$var[lblP]]$if[$var[p2]==N]$var[lbl2;$var[lblN]]$endif$if[$var[p2]==B]$var[lbl2;$var[lblB]]$endif$if[$var[p2]==R]$var[lbl2;$var[lblR]]$endif$if[$var[p2]==Q]$var[lbl2;$var[lblQ]]$endif$if[$var[p2]==K]$var[lbl2;$var[lblK]]$endif$addStringSelectOption[$var[lbl2] en $var[from2];$var[from2];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;3;from]!=]$var[p3;$httpResult[moves_by_square;3;piece]]$var[from3;$httpResult[moves_by_square;3;from]]$var[lbl3;$var[lblP]]$if[$var[p3]==N]$var[lbl3;$var[lblN]]$endif$if[$var[p3]==B]$var[lbl3;$var[lblB]]$endif$if[$var[p3]==R]$var[lbl3;$var[lblR]]$endif$if[$var[p3]==Q]$var[lbl3;$var[lblQ]]$endif$if[$var[p3]==K]$var[lbl3;$var[lblK]]$endif$addStringSelectOption[$var[lbl3] en $var[from3];$var[from3];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;4;from]!=]$var[p4;$httpResult[moves_by_square;4;piece]]$var[from4;$httpResult[moves_by_square;4;from]]$var[lbl4;$var[lblP]]$if[$var[p4]==N]$var[lbl4;$var[lblN]]$endif$if[$var[p4]==B]$var[lbl4;$var[lblB]]$endif$if[$var[p4]==R]$var[lbl4;$var[lblR]]$endif$if[$var[p4]==Q]$var[lbl4;$var[lblQ]]$endif$if[$var[p4]==K]$var[lbl4;$var[lblK]]$endif$addStringSelectOption[$var[lbl4] en $var[from4];$var[from4];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;5;from]!=]$var[p5;$httpResult[moves_by_square;5;piece]]$var[from5;$httpResult[moves_by_square;5;from]]$var[lbl5;$var[lblP]]$if[$var[p5]==N]$var[lbl5;$var[lblN]]$endif$if[$var[p5]==B]$var[lbl5;$var[lblB]]$endif$if[$var[p5]==R]$var[lbl5;$var[lblR]]$endif$if[$var[p5]==Q]$var[lbl5;$var[lblQ]]$endif$if[$var[p5]==K]$var[lbl5;$var[lblK]]$endif$addStringSelectOption[$var[lbl5] en $var[from5];$var[from5];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;6;from]!=]$var[p6;$httpResult[moves_by_square;6;piece]]$var[from6;$httpResult[moves_by_square;6;from]]$var[lbl6;$var[lblP]]$if[$var[p6]==N]$var[lbl6;$var[lblN]]$endif$if[$var[p6]==B]$var[lbl6;$var[lblB]]$endif$if[$var[p6]==R]$var[lbl6;$var[lblR]]$endif$if[$var[p6]==Q]$var[lbl6;$var[lblQ]]$endif$if[$var[p6]==K]$var[lbl6;$var[lblK]]$endif$addStringSelectOption[$var[lbl6] en $var[from6];$var[from6];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;7;from]!=]$var[p7;$httpResult[moves_by_square;7;piece]]$var[from7;$httpResult[moves_by_square;7;from]]$var[lbl7;$var[lblP]]$if[$var[p7]==N]$var[lbl7;$var[lblN]]$endif$if[$var[p7]==B]$var[lbl7;$var[lblB]]$endif$if[$var[p7]==R]$var[lbl7;$var[lblR]]$endif$if[$var[p7]==Q]$var[lbl7;$var[lblQ]]$endif$if[$var[p7]==K]$var[lbl7;$var[lblK]]$endif$addStringSelectOption[$var[lbl7] en $var[from7];$var[from7];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;8;from]!=]$var[p8;$httpResult[moves_by_square;8;piece]]$var[from8;$httpResult[moves_by_square;8;from]]$var[lbl8;$var[lblP]]$if[$var[p8]==N]$var[lbl8;$var[lblN]]$endif$if[$var[p8]==B]$var[lbl8;$var[lblB]]$endif$if[$var[p8]==R]$var[lbl8;$var[lblR]]$endif$if[$var[p8]==Q]$var[lbl8;$var[lblQ]]$endif$if[$var[p8]==K]$var[lbl8;$var[lblK]]$endif$addStringSelectOption[$var[lbl8] en $var[from8];$var[from8];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;9;from]!=]$var[p9;$httpResult[moves_by_square;9;piece]]$var[from9;$httpResult[moves_by_square;9;from]]$var[lbl9;$var[lblP]]$if[$var[p9]==N]$var[lbl9;$var[lblN]]$endif$if[$var[p9]==B]$var[lbl9;$var[lblB]]$endif$if[$var[p9]==R]$var[lbl9;$var[lblR]]$endif$if[$var[p9]==Q]$var[lbl9;$var[lblQ]]$endif$if[$var[p9]==K]$var[lbl9;$var[lblK]]$endif$addStringSelectOption[$var[lbl9] en $var[from9];$var[from9];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;10;from]!=]$var[p10;$httpResult[moves_by_square;10;piece]]$var[from10;$httpResult[moves_by_square;10;from]]$var[lbl10;$var[lblP]]$if[$var[p10]==N]$var[lbl10;$var[lblN]]$endif$if[$var[p10]==B]$var[lbl10;$var[lblB]]$endif$if[$var[p10]==R]$var[lbl10;$var[lblR]]$endif$if[$var[p10]==Q]$var[lbl10;$var[lblQ]]$endif$if[$var[p10]==K]$var[lbl10;$var[lblK]]$endif$addStringSelectOption[$var[lbl10] en $var[from10];$var[from10];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;11;from]!=]$var[p11;$httpResult[moves_by_square;11;piece]]$var[from11;$httpResult[moves_by_square;11;from]]$var[lbl11;$var[lblP]]$if[$var[p11]==N]$var[lbl11;$var[lblN]]$endif$if[$var[p11]==B]$var[lbl11;$var[lblB]]$endif$if[$var[p11]==R]$var[lbl11;$var[lblR]]$endif$if[$var[p11]==Q]$var[lbl11;$var[lblQ]]$endif$if[$var[p11]==K]$var[lbl11;$var[lblK]]$endif$addStringSelectOption[$var[lbl11] en $var[from11];$var[from11];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;12;from]!=]$var[p12;$httpResult[moves_by_square;12;piece]]$var[from12;$httpResult[moves_by_square;12;from]]$var[lbl12;$var[lblP]]$if[$var[p12]==N]$var[lbl12;$var[lblN]]$endif$if[$var[p12]==B]$var[lbl12;$var[lblB]]$endif$if[$var[p12]==R]$var[lbl12;$var[lblR]]$endif$if[$var[p12]==Q]$var[lbl12;$var[lblQ]]$endif$if[$var[p12]==K]$var[lbl12;$var[lblK]]$endif$addStringSelectOption[$var[lbl12] en $var[from12];$var[from12];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;13;from]!=]$var[p13;$httpResult[moves_by_square;13;piece]]$var[from13;$httpResult[moves_by_square;13;from]]$var[lbl13;$var[lblP]]$if[$var[p13]==N]$var[lbl13;$var[lblN]]$endif$if[$var[p13]==B]$var[lbl13;$var[lblB]]$endif$if[$var[p13]==R]$var[lbl13;$var[lblR]]$endif$if[$var[p13]==Q]$var[lbl13;$var[lblQ]]$endif$if[$var[p13]==K]$var[lbl13;$var[lblK]]$endif$addStringSelectOption[$var[lbl13] en $var[from13];$var[from13];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;14;from]!=]$var[p14;$httpResult[moves_by_square;14;piece]]$var[from14;$httpResult[moves_by_square;14;from]]$var[lbl14;$var[lblP]]$if[$var[p14]==N]$var[lbl14;$var[lblN]]$endif$if[$var[p14]==B]$var[lbl14;$var[lblB]]$endif$if[$var[p14]==R]$var[lbl14;$var[lblR]]$endif$if[$var[p14]==Q]$var[lbl14;$var[lblQ]]$endif$if[$var[p14]==K]$var[lbl14;$var[lblK]]$endif$addStringSelectOption[$var[lbl14] en $var[from14];$var[from14];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;15;from]!=]$var[p15;$httpResult[moves_by_square;15;piece]]$var[from15;$httpResult[moves_by_square;15;from]]$var[lbl15;$var[lblP]]$if[$var[p15]==N]$var[lbl15;$var[lblN]]$endif$if[$var[p15]==B]$var[lbl15;$var[lblB]]$endif$if[$var[p15]==R]$var[lbl15;$var[lblR]]$endif$if[$var[p15]==Q]$var[lbl15;$var[lblQ]]$endif$if[$var[p15]==K]$var[lbl15;$var[lblK]]$endif$addStringSelectOption[$var[lbl15] en $var[from15];$var[from15];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Rendirse;danger;false;;ctrl]
$addButtonCV2[chdraw~$var[wID]~$var[gID];🤝 Ofrecer tablas;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Jugadores:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chdecline]
$var[wID;$splitText[2]]
$var[bID;$splitText[3]]
$var[gID;$splitText[4]]

$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ Solo <@$var[bID]> puede rechazar este reto.]
$stop
$endif

$defer
$setVar[chess_state;;$var[wID]]
$removeAllComponents
$addContainer[main;#ED4245;false]
$addTextDisplay[# ♟️ **Reto rechazado**

<@$var[bID]> rechazó el reto de <@$var[wID]>.;main]
$stop
$endif

$if[$var[action]==chcancel]
$var[wID;$splitText[2]]
$var[bID;$splitText[3]]
$var[gID;$splitText[4]]

$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Solo <@$var[wID]> puede cancelar este reto.]
$stop
$endif

$defer
$setVar[chess_state;;$var[wID]]
$removeAllComponents
$addContainer[main;#ED4245;false]
$addTextDisplay[# ♟️ **Reto cancelado**

<@$var[wID]> canceló el reto.;main]
$stop
$endif

$if[$var[action]==chnew]
$var[oldwID;$splitText[2]]
$var[oldbID;$splitText[3]]

$if[$authorID!=$var[oldwID]]
$if[$authorID!=$var[oldbID]]
$ephemeral
$addTextDisplay[❌ Solo los jugadores de esta partida pueden iniciar una nueva.]
$stop
$endif
$endif

$var[existingState;$getVar[chess_state;$var[oldwID]]]
$if[$var[existingState]!=]
$textSplit[$var[existingState];|]
$var[existingStatus;$splitText[5]]
$if[$var[existingStatus]==p]
$ephemeral
$addTextDisplay[❌ Ya tienes una partida en curso. Termínala o ríndete antes de crear otra.]
$stop
$endif
$if[$var[existingStatus]==c]
$ephemeral
$addTextDisplay[❌ Ya tienes un reto pendiente.]
$stop
$endif
$endif

$defer
$var[gameID;$randomString[10]]
$var[theme;green]
$var[state;rnbqkbnr/pppppppp/8/8/8/8/PPPPPPPP/RNBQKBNR w KQkq - 0 1|$var[oldwID]|$var[oldbID]|$var[gameID]|c|-|-|$var[theme]]
$setVar[chess_state;$var[state];$var[oldwID]]

$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Ajedrez — Nueva partida**

<@$var[oldwID]> (♔ Blancas) desafía a <@$var[oldbID]> (♚ Negras) a una nueva partida de ajedrez.

⏳ Esperando respuesta de <@$var[oldbID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[oldwID]~$var[oldbID]~$var[gameID];✅ Aceptar;success;false;;r1]
$addButtonCV2[chdecline~$var[oldwID]~$var[oldbID]~$var[gameID];❌ Rechazar;danger;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**ID de partida:** \`$var[gameID]\`;main]
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
$addTextDisplay[❌ Solo los jugadores de esta partida pueden cerrarla.]
$stop
$endif

$defer
$setVar[chess_state;;$var[wID]]
$removeAllComponents
$addContainer[main;#2B2D31;false]
$addTextDisplay[# ♟️ **Ajedrez — Cerrado**

La partida fue cerrada.;main]
$stop
$endif

$if[$var[action]==chbp]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ No tienes partida activa. Ejecuta \`$commandTrigger\` para crear una.]
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

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Esta partida ya no está activa. Ejecuta \`$commandTrigger\` para crear una nueva.]
$stop
$endif

$if[$var[curStatus]==c]
$ephemeral
$addTextDisplay[ℹ️ El reto aún no fue aceptado. Espera a que el oponente acepte.]
$stop
$endif

$if[$var[curStatus]==do]
$ephemeral
$addTextDisplay[ℹ️ Hay una oferta de tablas pendiente. Respóndela primero.]
$stop
$endif

$if[$var[curStatus]!=p]
$ephemeral
$addTextDisplay[ℹ️ Esta partida ya terminó. Usa 🎲 Nueva partida para jugar otra.]
$stop
$endif

$textSplit[$var[fen]; ]
$var[turn;$splitText[2]]
$var[expectedPlayer;$var[wID]]
$if[$var[turn]==b]$var[expectedPlayer;$var[bID]]$endif
$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ No eres jugador de esta partida.]
$stop
$endif
$endif
$if[$authorID!=$var[expectedPlayer]]
$ephemeral
$addTextDisplay[❌ No es tu turno. Espera a que el otro jugador mueva.]
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
$var[turnLabel;♔ Blancas]
$var[activePlayer;$var[wID]]
$var[perspective;white]
$if[$var[turn]==b]
$var[turnLabel;♚ Negras]
$var[activePlayer;$var[bID]]
$endif
$var[lblP;♙ Peón]$var[lblN;♘ Caballo]$var[lblB;♗ Alfil]$var[lblR;♖ Torre]$var[lblQ;♕ Dama]$var[lblK;♔ Rey]$if[$var[turn]==b]$var[lblP;♟ Peón]$var[lblN;♞ Caballo]$var[lblB;♝ Alfil]$var[lblR;♜ Torre]$var[lblQ;♛ Dama]$var[lblK;♚ Rey]$endif
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Ajedrez**

**Turno:** $var[turnLabel]
**Mueve:** <@$var[activePlayer]>
**Última jugada:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Tablero de ajedrez;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Tu turno, <@$var[activePlayer]>.** Selecciona una pieza para mover:;main]
$addActionRow[r1;main]
$addStringSelect[chpiece~$var[wID]~$var[gID];Selecciona pieza...;1;1;;r1]
$if[$httpResult[moves_by_square;0;from]!=]$var[p0;$httpResult[moves_by_square;0;piece]]$var[from0;$httpResult[moves_by_square;0;from]]$var[lbl0;$var[lblP]]$if[$var[p0]==N]$var[lbl0;$var[lblN]]$endif$if[$var[p0]==B]$var[lbl0;$var[lblB]]$endif$if[$var[p0]==R]$var[lbl0;$var[lblR]]$endif$if[$var[p0]==Q]$var[lbl0;$var[lblQ]]$endif$if[$var[p0]==K]$var[lbl0;$var[lblK]]$endif$addStringSelectOption[$var[lbl0] en $var[from0];$var[from0];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;1;from]!=]$var[p1;$httpResult[moves_by_square;1;piece]]$var[from1;$httpResult[moves_by_square;1;from]]$var[lbl1;$var[lblP]]$if[$var[p1]==N]$var[lbl1;$var[lblN]]$endif$if[$var[p1]==B]$var[lbl1;$var[lblB]]$endif$if[$var[p1]==R]$var[lbl1;$var[lblR]]$endif$if[$var[p1]==Q]$var[lbl1;$var[lblQ]]$endif$if[$var[p1]==K]$var[lbl1;$var[lblK]]$endif$addStringSelectOption[$var[lbl1] en $var[from1];$var[from1];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;2;from]!=]$var[p2;$httpResult[moves_by_square;2;piece]]$var[from2;$httpResult[moves_by_square;2;from]]$var[lbl2;$var[lblP]]$if[$var[p2]==N]$var[lbl2;$var[lblN]]$endif$if[$var[p2]==B]$var[lbl2;$var[lblB]]$endif$if[$var[p2]==R]$var[lbl2;$var[lblR]]$endif$if[$var[p2]==Q]$var[lbl2;$var[lblQ]]$endif$if[$var[p2]==K]$var[lbl2;$var[lblK]]$endif$addStringSelectOption[$var[lbl2] en $var[from2];$var[from2];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;3;from]!=]$var[p3;$httpResult[moves_by_square;3;piece]]$var[from3;$httpResult[moves_by_square;3;from]]$var[lbl3;$var[lblP]]$if[$var[p3]==N]$var[lbl3;$var[lblN]]$endif$if[$var[p3]==B]$var[lbl3;$var[lblB]]$endif$if[$var[p3]==R]$var[lbl3;$var[lblR]]$endif$if[$var[p3]==Q]$var[lbl3;$var[lblQ]]$endif$if[$var[p3]==K]$var[lbl3;$var[lblK]]$endif$addStringSelectOption[$var[lbl3] en $var[from3];$var[from3];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;4;from]!=]$var[p4;$httpResult[moves_by_square;4;piece]]$var[from4;$httpResult[moves_by_square;4;from]]$var[lbl4;$var[lblP]]$if[$var[p4]==N]$var[lbl4;$var[lblN]]$endif$if[$var[p4]==B]$var[lbl4;$var[lblB]]$endif$if[$var[p4]==R]$var[lbl4;$var[lblR]]$endif$if[$var[p4]==Q]$var[lbl4;$var[lblQ]]$endif$if[$var[p4]==K]$var[lbl4;$var[lblK]]$endif$addStringSelectOption[$var[lbl4] en $var[from4];$var[from4];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;5;from]!=]$var[p5;$httpResult[moves_by_square;5;piece]]$var[from5;$httpResult[moves_by_square;5;from]]$var[lbl5;$var[lblP]]$if[$var[p5]==N]$var[lbl5;$var[lblN]]$endif$if[$var[p5]==B]$var[lbl5;$var[lblB]]$endif$if[$var[p5]==R]$var[lbl5;$var[lblR]]$endif$if[$var[p5]==Q]$var[lbl5;$var[lblQ]]$endif$if[$var[p5]==K]$var[lbl5;$var[lblK]]$endif$addStringSelectOption[$var[lbl5] en $var[from5];$var[from5];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;6;from]!=]$var[p6;$httpResult[moves_by_square;6;piece]]$var[from6;$httpResult[moves_by_square;6;from]]$var[lbl6;$var[lblP]]$if[$var[p6]==N]$var[lbl6;$var[lblN]]$endif$if[$var[p6]==B]$var[lbl6;$var[lblB]]$endif$if[$var[p6]==R]$var[lbl6;$var[lblR]]$endif$if[$var[p6]==Q]$var[lbl6;$var[lblQ]]$endif$if[$var[p6]==K]$var[lbl6;$var[lblK]]$endif$addStringSelectOption[$var[lbl6] en $var[from6];$var[from6];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;7;from]!=]$var[p7;$httpResult[moves_by_square;7;piece]]$var[from7;$httpResult[moves_by_square;7;from]]$var[lbl7;$var[lblP]]$if[$var[p7]==N]$var[lbl7;$var[lblN]]$endif$if[$var[p7]==B]$var[lbl7;$var[lblB]]$endif$if[$var[p7]==R]$var[lbl7;$var[lblR]]$endif$if[$var[p7]==Q]$var[lbl7;$var[lblQ]]$endif$if[$var[p7]==K]$var[lbl7;$var[lblK]]$endif$addStringSelectOption[$var[lbl7] en $var[from7];$var[from7];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;8;from]!=]$var[p8;$httpResult[moves_by_square;8;piece]]$var[from8;$httpResult[moves_by_square;8;from]]$var[lbl8;$var[lblP]]$if[$var[p8]==N]$var[lbl8;$var[lblN]]$endif$if[$var[p8]==B]$var[lbl8;$var[lblB]]$endif$if[$var[p8]==R]$var[lbl8;$var[lblR]]$endif$if[$var[p8]==Q]$var[lbl8;$var[lblQ]]$endif$if[$var[p8]==K]$var[lbl8;$var[lblK]]$endif$addStringSelectOption[$var[lbl8] en $var[from8];$var[from8];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;9;from]!=]$var[p9;$httpResult[moves_by_square;9;piece]]$var[from9;$httpResult[moves_by_square;9;from]]$var[lbl9;$var[lblP]]$if[$var[p9]==N]$var[lbl9;$var[lblN]]$endif$if[$var[p9]==B]$var[lbl9;$var[lblB]]$endif$if[$var[p9]==R]$var[lbl9;$var[lblR]]$endif$if[$var[p9]==Q]$var[lbl9;$var[lblQ]]$endif$if[$var[p9]==K]$var[lbl9;$var[lblK]]$endif$addStringSelectOption[$var[lbl9] en $var[from9];$var[from9];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;10;from]!=]$var[p10;$httpResult[moves_by_square;10;piece]]$var[from10;$httpResult[moves_by_square;10;from]]$var[lbl10;$var[lblP]]$if[$var[p10]==N]$var[lbl10;$var[lblN]]$endif$if[$var[p10]==B]$var[lbl10;$var[lblB]]$endif$if[$var[p10]==R]$var[lbl10;$var[lblR]]$endif$if[$var[p10]==Q]$var[lbl10;$var[lblQ]]$endif$if[$var[p10]==K]$var[lbl10;$var[lblK]]$endif$addStringSelectOption[$var[lbl10] en $var[from10];$var[from10];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;11;from]!=]$var[p11;$httpResult[moves_by_square;11;piece]]$var[from11;$httpResult[moves_by_square;11;from]]$var[lbl11;$var[lblP]]$if[$var[p11]==N]$var[lbl11;$var[lblN]]$endif$if[$var[p11]==B]$var[lbl11;$var[lblB]]$endif$if[$var[p11]==R]$var[lbl11;$var[lblR]]$endif$if[$var[p11]==Q]$var[lbl11;$var[lblQ]]$endif$if[$var[p11]==K]$var[lbl11;$var[lblK]]$endif$addStringSelectOption[$var[lbl11] en $var[from11];$var[from11];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;12;from]!=]$var[p12;$httpResult[moves_by_square;12;piece]]$var[from12;$httpResult[moves_by_square;12;from]]$var[lbl12;$var[lblP]]$if[$var[p12]==N]$var[lbl12;$var[lblN]]$endif$if[$var[p12]==B]$var[lbl12;$var[lblB]]$endif$if[$var[p12]==R]$var[lbl12;$var[lblR]]$endif$if[$var[p12]==Q]$var[lbl12;$var[lblQ]]$endif$if[$var[p12]==K]$var[lbl12;$var[lblK]]$endif$addStringSelectOption[$var[lbl12] en $var[from12];$var[from12];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;13;from]!=]$var[p13;$httpResult[moves_by_square;13;piece]]$var[from13;$httpResult[moves_by_square;13;from]]$var[lbl13;$var[lblP]]$if[$var[p13]==N]$var[lbl13;$var[lblN]]$endif$if[$var[p13]==B]$var[lbl13;$var[lblB]]$endif$if[$var[p13]==R]$var[lbl13;$var[lblR]]$endif$if[$var[p13]==Q]$var[lbl13;$var[lblQ]]$endif$if[$var[p13]==K]$var[lbl13;$var[lblK]]$endif$addStringSelectOption[$var[lbl13] en $var[from13];$var[from13];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;14;from]!=]$var[p14;$httpResult[moves_by_square;14;piece]]$var[from14;$httpResult[moves_by_square;14;from]]$var[lbl14;$var[lblP]]$if[$var[p14]==N]$var[lbl14;$var[lblN]]$endif$if[$var[p14]==B]$var[lbl14;$var[lblB]]$endif$if[$var[p14]==R]$var[lbl14;$var[lblR]]$endif$if[$var[p14]==Q]$var[lbl14;$var[lblQ]]$endif$if[$var[p14]==K]$var[lbl14;$var[lblK]]$endif$addStringSelectOption[$var[lbl14] en $var[from14];$var[from14];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;15;from]!=]$var[p15;$httpResult[moves_by_square;15;piece]]$var[from15;$httpResult[moves_by_square;15;from]]$var[lbl15;$var[lblP]]$if[$var[p15]==N]$var[lbl15;$var[lblN]]$endif$if[$var[p15]==B]$var[lbl15;$var[lblB]]$endif$if[$var[p15]==R]$var[lbl15;$var[lblR]]$endif$if[$var[p15]==Q]$var[lbl15;$var[lblQ]]$endif$if[$var[p15]==K]$var[lbl15;$var[lblK]]$endif$addStringSelectOption[$var[lbl15] en $var[from15];$var[from15];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Rendirse;danger;false;;ctrl]
$addButtonCV2[chdraw~$var[wID]~$var[gID];🤝 Ofrecer tablas;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Jugadores:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chforfeit]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ No tienes partida activa. Ejecuta \`$commandTrigger\` para crear una.]
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

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Esta partida ya no está activa.]
$stop
$endif

$if[$var[curStatus]==c]
$ephemeral
$addTextDisplay[ℹ️ El reto aún no fue aceptado. Usa el botón 🚪 Cancelar para cancelarlo.]
$stop
$endif

$if[$var[curStatus]!=p]
$if[$var[curStatus]!=do]
$ephemeral
$addTextDisplay[ℹ️ Esta partida ya terminó. Usa 🎲 Nueva partida para jugar otra.]
$stop
$endif
$endif

$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ No eres jugador de esta partida. Ejecuta \`$commandTrigger\` para crear la tuya.]
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
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|$var[finalStatus]|-|Rendición|$var[theme]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[lastMoveSAN;Rendición]
$removeAllComponents
$var[cc;#ED4245]
$var[resultMsg;🚪 Partida abandonada]
$if[$var[finalStatus]==w]
$var[cc;#57F287]
$var[resultMsg;🏆 ¡Ganaron las Blancas! <@$var[wID]>]
$endif
$if[$var[finalStatus]==b]
$var[cc;#57F287]
$var[resultMsg;🏆 ¡Ganaron las Negras! <@$var[bID]>]
$endif
$if[$var[finalStatus]==d]
$var[cc;#FEE75C]
$var[resultMsg;🤝 ¡Tablas! Partida empatada]
$endif
$var[perspective;white]
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Ajedrez — Partida finalizada**

$var[resultMsg]

**Última jugada:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Tablero final;false;board]
$addSeparator[true;small;main]
$addTextDisplay[📊 **Resultado:** $var[resultMsg];main]
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chnew~$var[wID]~$var[bID];🎲 Nueva partida;success;false;;ctrl]
$addButtonCV2[chclose~$var[wID]~$var[gID];🗑️ Cerrar;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Jugadores:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chdraw]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ No tienes partida activa. Ejecuta \`$commandTrigger\` para crear una.]
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

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Esta partida ya no está activa.]
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
$addTextDisplay[ℹ️ Tú ofreciste tablas. Espera la respuesta de tu oponente.]
$stop
$endif
$if[$authorID!=$var[responder]]
$ephemeral
$addTextDisplay[❌ Solo el oponente puede responder a esta oferta.]
$stop
$endif

$defer
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|d|-|Tablas acordadas|$var[theme]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[finalStatus;d]
$var[lastMoveSAN;Tablas]
$removeAllComponents
$var[cc;#ED4245]
$var[resultMsg;🚪 Partida abandonada]
$if[$var[finalStatus]==w]
$var[cc;#57F287]
$var[resultMsg;🏆 ¡Ganaron las Blancas! <@$var[wID]>]
$endif
$if[$var[finalStatus]==b]
$var[cc;#57F287]
$var[resultMsg;🏆 ¡Ganaron las Negras! <@$var[bID]>]
$endif
$if[$var[finalStatus]==d]
$var[cc;#FEE75C]
$var[resultMsg;🤝 ¡Tablas! Partida empatada]
$endif
$var[perspective;white]
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Ajedrez — Partida finalizada**

$var[resultMsg]

**Última jugada:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Tablero final;false;board]
$addSeparator[true;small;main]
$addTextDisplay[📊 **Resultado:** $var[resultMsg];main]
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chnew~$var[wID]~$var[bID];🎲 Nueva partida;success;false;;ctrl]
$addButtonCV2[chclose~$var[wID]~$var[gID];🗑️ Cerrar;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Jugadores:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[curStatus]!=p]
$ephemeral
$addTextDisplay[ℹ️ Esta partida ya terminó. Usa 🎲 Nueva partida para jugar otra.]
$stop
$endif

$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ No eres jugador de esta partida. Ejecuta \`$commandTrigger\` para crear la tuya.]
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
$addTextDisplay[❌ No es tu turno para ofrecer tablas.]
$stop
$endif

$defer
$var[perspective;white]

$var[lastMoveSAN2;-]
$textSplit[$var[state];|]
$var[lastMoveSAN2;$splitText[7]]

$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|do|-|$var[lastMoveSAN2]|$var[theme]]
$setVar[chess_state;$var[newState];$var[wID]]

$removeAllComponents
$addContainer[main;#FEE75C;false]
$addTextDisplay[# ♟️ **Ajedrez — Oferta de tablas**

<@$var[expectedPlayer]> ofrece tablas a <@$var[opponentPlayer]>.

¿Aceptas las tablas?;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme];Tablero actual;false;board]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chdraw~$var[wID]~$var[gID];✅ Aceptar tablas;success;false;;r1]
$addButtonCV2[chdrawdecline~$var[wID]~$var[gID];❌ Rechazar tablas;danger;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**Jugadores:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chdrawdecline]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ No tienes partida activa. Ejecuta \`$commandTrigger\` para crear una.]
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

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Esta partida ya no está activa.]
$stop
$endif

$if[$var[curStatus]!=do]
$ephemeral
$addTextDisplay[ℹ️ No hay oferta de tablas pendiente.]
$stop
$endif

$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ No eres jugador de esta partida. Ejecuta \`$commandTrigger\` para crear la tuya.]
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
$addTextDisplay[❌ Solo el oponente puede responder a esta oferta.]
$stop
$endif

$defer
$var[lastMoveSAN;-]
$textSplit[$var[state];|]
$var[lastMoveSAN;$splitText[7]]

$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|p|-|$var[lastMoveSAN]|$var[theme]]
$setVar[chess_state;$var[newState];$var[wID]]

$removeAllComponents
$httpGet[http://chess.nexusify.co/moves?fen=$url[encode;$var[fen]]]
$var[movesCount;$httpResult[count]]
$var[turn;$httpResult[turn]]
$var[cc;#5865F2]
$var[turnLabel;♔ Blancas]
$var[activePlayer;$var[wID]]
$var[perspective;white]
$if[$var[turn]==b]
$var[turnLabel;♚ Negras]
$var[activePlayer;$var[bID]]
$endif
$var[lblP;♙ Peón]$var[lblN;♘ Caballo]$var[lblB;♗ Alfil]$var[lblR;♖ Torre]$var[lblQ;♕ Dama]$var[lblK;♔ Rey]$if[$var[turn]==b]$var[lblP;♟ Peón]$var[lblN;♞ Caballo]$var[lblB;♝ Alfil]$var[lblR;♜ Torre]$var[lblQ;♛ Dama]$var[lblK;♚ Rey]$endif
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Ajedrez**

**Turno:** $var[turnLabel]
**Mueve:** <@$var[activePlayer]>
**Última jugada:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Tablero de ajedrez;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Tu turno, <@$var[activePlayer]>.** Selecciona una pieza para mover:;main]
$addActionRow[r1;main]
$addStringSelect[chpiece~$var[wID]~$var[gID];Selecciona pieza...;1;1;;r1]
$if[$httpResult[moves_by_square;0;from]!=]$var[p0;$httpResult[moves_by_square;0;piece]]$var[from0;$httpResult[moves_by_square;0;from]]$var[lbl0;$var[lblP]]$if[$var[p0]==N]$var[lbl0;$var[lblN]]$endif$if[$var[p0]==B]$var[lbl0;$var[lblB]]$endif$if[$var[p0]==R]$var[lbl0;$var[lblR]]$endif$if[$var[p0]==Q]$var[lbl0;$var[lblQ]]$endif$if[$var[p0]==K]$var[lbl0;$var[lblK]]$endif$addStringSelectOption[$var[lbl0] en $var[from0];$var[from0];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;1;from]!=]$var[p1;$httpResult[moves_by_square;1;piece]]$var[from1;$httpResult[moves_by_square;1;from]]$var[lbl1;$var[lblP]]$if[$var[p1]==N]$var[lbl1;$var[lblN]]$endif$if[$var[p1]==B]$var[lbl1;$var[lblB]]$endif$if[$var[p1]==R]$var[lbl1;$var[lblR]]$endif$if[$var[p1]==Q]$var[lbl1;$var[lblQ]]$endif$if[$var[p1]==K]$var[lbl1;$var[lblK]]$endif$addStringSelectOption[$var[lbl1] en $var[from1];$var[from1];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;2;from]!=]$var[p2;$httpResult[moves_by_square;2;piece]]$var[from2;$httpResult[moves_by_square;2;from]]$var[lbl2;$var[lblP]]$if[$var[p2]==N]$var[lbl2;$var[lblN]]$endif$if[$var[p2]==B]$var[lbl2;$var[lblB]]$endif$if[$var[p2]==R]$var[lbl2;$var[lblR]]$endif$if[$var[p2]==Q]$var[lbl2;$var[lblQ]]$endif$if[$var[p2]==K]$var[lbl2;$var[lblK]]$endif$addStringSelectOption[$var[lbl2] en $var[from2];$var[from2];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;3;from]!=]$var[p3;$httpResult[moves_by_square;3;piece]]$var[from3;$httpResult[moves_by_square;3;from]]$var[lbl3;$var[lblP]]$if[$var[p3]==N]$var[lbl3;$var[lblN]]$endif$if[$var[p3]==B]$var[lbl3;$var[lblB]]$endif$if[$var[p3]==R]$var[lbl3;$var[lblR]]$endif$if[$var[p3]==Q]$var[lbl3;$var[lblQ]]$endif$if[$var[p3]==K]$var[lbl3;$var[lblK]]$endif$addStringSelectOption[$var[lbl3] en $var[from3];$var[from3];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;4;from]!=]$var[p4;$httpResult[moves_by_square;4;piece]]$var[from4;$httpResult[moves_by_square;4;from]]$var[lbl4;$var[lblP]]$if[$var[p4]==N]$var[lbl4;$var[lblN]]$endif$if[$var[p4]==B]$var[lbl4;$var[lblB]]$endif$if[$var[p4]==R]$var[lbl4;$var[lblR]]$endif$if[$var[p4]==Q]$var[lbl4;$var[lblQ]]$endif$if[$var[p4]==K]$var[lbl4;$var[lblK]]$endif$addStringSelectOption[$var[lbl4] en $var[from4];$var[from4];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;5;from]!=]$var[p5;$httpResult[moves_by_square;5;piece]]$var[from5;$httpResult[moves_by_square;5;from]]$var[lbl5;$var[lblP]]$if[$var[p5]==N]$var[lbl5;$var[lblN]]$endif$if[$var[p5]==B]$var[lbl5;$var[lblB]]$endif$if[$var[p5]==R]$var[lbl5;$var[lblR]]$endif$if[$var[p5]==Q]$var[lbl5;$var[lblQ]]$endif$if[$var[p5]==K]$var[lbl5;$var[lblK]]$endif$addStringSelectOption[$var[lbl5] en $var[from5];$var[from5];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;6;from]!=]$var[p6;$httpResult[moves_by_square;6;piece]]$var[from6;$httpResult[moves_by_square;6;from]]$var[lbl6;$var[lblP]]$if[$var[p6]==N]$var[lbl6;$var[lblN]]$endif$if[$var[p6]==B]$var[lbl6;$var[lblB]]$endif$if[$var[p6]==R]$var[lbl6;$var[lblR]]$endif$if[$var[p6]==Q]$var[lbl6;$var[lblQ]]$endif$if[$var[p6]==K]$var[lbl6;$var[lblK]]$endif$addStringSelectOption[$var[lbl6] en $var[from6];$var[from6];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;7;from]!=]$var[p7;$httpResult[moves_by_square;7;piece]]$var[from7;$httpResult[moves_by_square;7;from]]$var[lbl7;$var[lblP]]$if[$var[p7]==N]$var[lbl7;$var[lblN]]$endif$if[$var[p7]==B]$var[lbl7;$var[lblB]]$endif$if[$var[p7]==R]$var[lbl7;$var[lblR]]$endif$if[$var[p7]==Q]$var[lbl7;$var[lblQ]]$endif$if[$var[p7]==K]$var[lbl7;$var[lblK]]$endif$addStringSelectOption[$var[lbl7] en $var[from7];$var[from7];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;8;from]!=]$var[p8;$httpResult[moves_by_square;8;piece]]$var[from8;$httpResult[moves_by_square;8;from]]$var[lbl8;$var[lblP]]$if[$var[p8]==N]$var[lbl8;$var[lblN]]$endif$if[$var[p8]==B]$var[lbl8;$var[lblB]]$endif$if[$var[p8]==R]$var[lbl8;$var[lblR]]$endif$if[$var[p8]==Q]$var[lbl8;$var[lblQ]]$endif$if[$var[p8]==K]$var[lbl8;$var[lblK]]$endif$addStringSelectOption[$var[lbl8] en $var[from8];$var[from8];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;9;from]!=]$var[p9;$httpResult[moves_by_square;9;piece]]$var[from9;$httpResult[moves_by_square;9;from]]$var[lbl9;$var[lblP]]$if[$var[p9]==N]$var[lbl9;$var[lblN]]$endif$if[$var[p9]==B]$var[lbl9;$var[lblB]]$endif$if[$var[p9]==R]$var[lbl9;$var[lblR]]$endif$if[$var[p9]==Q]$var[lbl9;$var[lblQ]]$endif$if[$var[p9]==K]$var[lbl9;$var[lblK]]$endif$addStringSelectOption[$var[lbl9] en $var[from9];$var[from9];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;10;from]!=]$var[p10;$httpResult[moves_by_square;10;piece]]$var[from10;$httpResult[moves_by_square;10;from]]$var[lbl10;$var[lblP]]$if[$var[p10]==N]$var[lbl10;$var[lblN]]$endif$if[$var[p10]==B]$var[lbl10;$var[lblB]]$endif$if[$var[p10]==R]$var[lbl10;$var[lblR]]$endif$if[$var[p10]==Q]$var[lbl10;$var[lblQ]]$endif$if[$var[p10]==K]$var[lbl10;$var[lblK]]$endif$addStringSelectOption[$var[lbl10] en $var[from10];$var[from10];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;11;from]!=]$var[p11;$httpResult[moves_by_square;11;piece]]$var[from11;$httpResult[moves_by_square;11;from]]$var[lbl11;$var[lblP]]$if[$var[p11]==N]$var[lbl11;$var[lblN]]$endif$if[$var[p11]==B]$var[lbl11;$var[lblB]]$endif$if[$var[p11]==R]$var[lbl11;$var[lblR]]$endif$if[$var[p11]==Q]$var[lbl11;$var[lblQ]]$endif$if[$var[p11]==K]$var[lbl11;$var[lblK]]$endif$addStringSelectOption[$var[lbl11] en $var[from11];$var[from11];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;12;from]!=]$var[p12;$httpResult[moves_by_square;12;piece]]$var[from12;$httpResult[moves_by_square;12;from]]$var[lbl12;$var[lblP]]$if[$var[p12]==N]$var[lbl12;$var[lblN]]$endif$if[$var[p12]==B]$var[lbl12;$var[lblB]]$endif$if[$var[p12]==R]$var[lbl12;$var[lblR]]$endif$if[$var[p12]==Q]$var[lbl12;$var[lblQ]]$endif$if[$var[p12]==K]$var[lbl12;$var[lblK]]$endif$addStringSelectOption[$var[lbl12] en $var[from12];$var[from12];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;13;from]!=]$var[p13;$httpResult[moves_by_square;13;piece]]$var[from13;$httpResult[moves_by_square;13;from]]$var[lbl13;$var[lblP]]$if[$var[p13]==N]$var[lbl13;$var[lblN]]$endif$if[$var[p13]==B]$var[lbl13;$var[lblB]]$endif$if[$var[p13]==R]$var[lbl13;$var[lblR]]$endif$if[$var[p13]==Q]$var[lbl13;$var[lblQ]]$endif$if[$var[p13]==K]$var[lbl13;$var[lblK]]$endif$addStringSelectOption[$var[lbl13] en $var[from13];$var[from13];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;14;from]!=]$var[p14;$httpResult[moves_by_square;14;piece]]$var[from14;$httpResult[moves_by_square;14;from]]$var[lbl14;$var[lblP]]$if[$var[p14]==N]$var[lbl14;$var[lblN]]$endif$if[$var[p14]==B]$var[lbl14;$var[lblB]]$endif$if[$var[p14]==R]$var[lbl14;$var[lblR]]$endif$if[$var[p14]==Q]$var[lbl14;$var[lblQ]]$endif$if[$var[p14]==K]$var[lbl14;$var[lblK]]$endif$addStringSelectOption[$var[lbl14] en $var[from14];$var[from14];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;15;from]!=]$var[p15;$httpResult[moves_by_square;15;piece]]$var[from15;$httpResult[moves_by_square;15;from]]$var[lbl15;$var[lblP]]$if[$var[p15]==N]$var[lbl15;$var[lblN]]$endif$if[$var[p15]==B]$var[lbl15;$var[lblB]]$endif$if[$var[p15]==R]$var[lbl15;$var[lblR]]$endif$if[$var[p15]==Q]$var[lbl15;$var[lblQ]]$endif$if[$var[p15]==K]$var[lbl15;$var[lblK]]$endif$addStringSelectOption[$var[lbl15] en $var[from15];$var[from15];Pieza;;;chpiece~$var[wID]~$var[gID]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Rendirse;danger;false;;ctrl]
$addButtonCV2[chdraw~$var[wID]~$var[gID];🤝 Ofrecer tablas;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Jugadores:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chthemegr]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[newTheme;green]
$if[$authorID!=$var[wID]]
$ephemeral
$addTextDisplay[❌ Solo el retador (<@$var[wID]>) puede cambiar el tema del tablero.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ No tienes partida activa. Ejecuta \`$commandTrigger\` para crear una.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Esta partida ya no está activa. Ejecuta \`$commandTrigger\` para crear una nueva.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[ℹ️ Solo puedes cambiar el tema antes de que el reto sea aceptado.]
$stop
$endif

$defer
$var[theme;$var[newTheme]]
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|$var[curStatus]|-|-|$var[theme]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[challengerID;$var[wID]]
$var[opponentID;$var[bID]]
$var[gameID;$var[gID]]
$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Ajedrez — Reto pendiente**

<@$var[challengerID]> (♔ Blancas) desafía a <@$var[opponentID]> (♚ Negras) a una partida de ajedrez.

⏳ Esperando respuesta de <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Aceptar;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Rechazar;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Cancelar;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**ID de partida:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Elegir tablero** — Solo <@$var[challengerID]> puede cambiar el tema.
Tema actual: **$var[theme]**;themes]
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
$addTextDisplay[❌ Solo el retador (<@$var[wID]>) puede cambiar el tema del tablero.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ No tienes partida activa. Ejecuta \`$commandTrigger\` para crear una.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Esta partida ya no está activa. Ejecuta \`$commandTrigger\` para crear una nueva.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[ℹ️ Solo puedes cambiar el tema antes de que el reto sea aceptado.]
$stop
$endif

$defer
$var[theme;$var[newTheme]]
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|$var[curStatus]|-|-|$var[theme]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[challengerID;$var[wID]]
$var[opponentID;$var[bID]]
$var[gameID;$var[gID]]
$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Ajedrez — Reto pendiente**

<@$var[challengerID]> (♔ Blancas) desafía a <@$var[opponentID]> (♚ Negras) a una partida de ajedrez.

⏳ Esperando respuesta de <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Aceptar;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Rechazar;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Cancelar;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**ID de partida:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Elegir tablero** — Solo <@$var[challengerID]> puede cambiar el tema.
Tema actual: **$var[theme]**;themes]
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
$addTextDisplay[❌ Solo el retador (<@$var[wID]>) puede cambiar el tema del tablero.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ No tienes partida activa. Ejecuta \`$commandTrigger\` para crear una.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Esta partida ya no está activa. Ejecuta \`$commandTrigger\` para crear una nueva.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[ℹ️ Solo puedes cambiar el tema antes de que el reto sea aceptado.]
$stop
$endif

$defer
$var[theme;$var[newTheme]]
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|$var[curStatus]|-|-|$var[theme]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[challengerID;$var[wID]]
$var[opponentID;$var[bID]]
$var[gameID;$var[gID]]
$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Ajedrez — Reto pendiente**

<@$var[challengerID]> (♔ Blancas) desafía a <@$var[opponentID]> (♚ Negras) a una partida de ajedrez.

⏳ Esperando respuesta de <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Aceptar;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Rechazar;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Cancelar;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**ID de partida:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Elegir tablero** — Solo <@$var[challengerID]> puede cambiar el tema.
Tema actual: **$var[theme]**;themes]
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
$addTextDisplay[❌ Solo el retador (<@$var[wID]>) puede cambiar el tema del tablero.]
$stop
$endif

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ No tienes partida activa. Ejecuta \`$commandTrigger\` para crear una.]
$stop
$endif

$textSplit[$var[state];|]
$var[fen;$splitText[1]]
$var[bID;$splitText[3]]
$var[curGID;$splitText[4]]
$var[curStatus;$splitText[5]]

$if[$var[gID]!=$var[curGID]]
$ephemeral
$addTextDisplay[❌ Esta partida ya no está activa. Ejecuta \`$commandTrigger\` para crear una nueva.]
$stop
$endif

$if[$var[curStatus]!=c]
$ephemeral
$addTextDisplay[ℹ️ Solo puedes cambiar el tema antes de que el reto sea aceptado.]
$stop
$endif

$defer
$var[theme;$var[newTheme]]
$var[newState;$var[fen]|$var[wID]|$var[bID]|$var[gID]|$var[curStatus]|-|-|$var[theme]]
$setVar[chess_state;$var[newState];$var[wID]]

$var[challengerID;$var[wID]]
$var[opponentID;$var[bID]]
$var[gameID;$var[gID]]
$removeAllComponents
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Ajedrez — Reto pendiente**

<@$var[challengerID]> (♔ Blancas) desafía a <@$var[opponentID]> (♚ Negras) a una partida de ajedrez.

⏳ Esperando respuesta de <@$var[opponentID]>...;main]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addButtonCV2[chaccept~$var[challengerID]~$var[opponentID]~$var[gameID];✅ Aceptar;success;false;;r1]
$addButtonCV2[chdecline~$var[challengerID]~$var[opponentID]~$var[gameID];❌ Rechazar;danger;false;;r1]
$addButtonCV2[chcancel~$var[challengerID]~$var[opponentID]~$var[gameID];🚪 Cancelar;secondary;false;;r1]
$addSeparator[true;small;main]
$addTextDisplay[**ID de partida:** \`$var[gameID]\`;main]
$addSeparator[true;small]
$addContainer[themes;#2B2D31;false]
$addTextDisplay[🎨 **Elegir tablero** — Solo <@$var[challengerID]> puede cambiar el tema.
Tema actual: **$var[theme]**;themes]
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
```

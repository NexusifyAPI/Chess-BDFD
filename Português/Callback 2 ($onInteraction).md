# Gatilho:

```
$onInteraction
```

# Código:

```
$nomention
$disableInnerSpaceRemoval
$try

$if[$checkContains[$customID;chpiece~;chbpr~;chpr~;chdest~]==false]
$stop
$endif

$textSplit[$customID;~]
$var[action;$splitText[1]]

$if[$var[action]==chpiece]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[fromSquare;$message]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Você não tem partida ativa. Execute \`$var[trigger]\` para criar uma.]
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
$addTextDisplay[❌ Esta partida não está mais ativa. Execute \`$var[trigger]\` para criar uma nova.]
$stop
$endif

$if[$var[curStatus]==c]
$ephemeral
$addTextDisplay[ℹ️ O desafio ainda não foi aceito. Espere o oponente aceitar.]
$stop
$endif

$if[$var[curStatus]==do]
$ephemeral
$addTextDisplay[ℹ️ Há uma oferta de empate pendente. Responda-a primeiro.]
$stop
$endif

$if[$var[curStatus]!=p]
$ephemeral
$addTextDisplay[ℹ️ Esta partida já terminou. Use 🎲 Nova partida para jogar outra.]
$stop
$endif

$textSplit[$var[fen]; ]
$var[turn;$splitText[2]]
$var[expectedPlayer;$var[wID]]
$if[$var[turn]==b]$var[expectedPlayer;$var[bID]]$endif
$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ Você não é jogador desta partida.]
$stop
$endif
$endif
$if[$authorID!=$var[expectedPlayer]]
$ephemeral
$addTextDisplay[❌ Não é o seu turno. Espere o outro jogador jogar.]
$stop
$endif

$defer
$removeAllComponents
$httpGet[http://chess.nexusify.co/moves?fen=$url[encode;$var[fen]]&square=$var[fromSquare]]
$var[movesCount;$httpResult[count]]
$var[pieceLetter;$httpResult[piece]]
$var[pieceColor;$httpResult[piece_color]]
$var[turn;$var[pieceColor]]
$var[perspective;white]
$var[turnLabel;♔ Brancas]
$if[$var[turn]==b]
$var[turnLabel;♚ Pretas]
$endif
$var[lblP;♙ Peão]$var[lblN;♘ Cavalo]$var[lblB;♗ Bispo]$var[lblR;♖ Torre]$var[lblQ;♕ Dama]$var[lblK;♔ Rei]$if[$var[turn]==b]$var[lblP;♟ Peão]$var[lblN;♞ Cavalo]$var[lblB;♝ Bispo]$var[lblR;♜ Torre]$var[lblQ;♛ Dama]$var[lblK;♚ Rei]$endif
$var[pieceName;]
$if[$var[pieceLetter]==P]$var[pieceName;$var[lblP]]$endif
$if[$var[pieceLetter]==N]$var[pieceName;$var[lblN]]$endif
$if[$var[pieceLetter]==B]$var[pieceName;$var[lblB]]$endif
$if[$var[pieceLetter]==R]$var[pieceName;$var[lblR]]$endif
$if[$var[pieceLetter]==Q]$var[pieceName;$var[lblQ]]$endif
$if[$var[pieceLetter]==K]$var[pieceName;$var[lblK]]$endif
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Xadrez — Mover peça**

**Peça selecionada:** $var[pieceName] em \`$var[fromSquare]\`
**Turno:** $var[turnLabel];main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Tabuleiro de xadrez;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Para onde mover?** Selecione uma casa de destino:;main]
$addActionRow[r1;main]
$addStringSelect[chdest~$var[wID]~$var[gID]~$var[fromSquare];Selecionar destino...;1;1;;r1]
$if[$httpResult[moves;0]!=]$var[d0;$httpResult[moves;0]]$var[dp0;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d0];8]==true]$var[dp0;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d0];1]==true]$var[dp0;p]$endif$endif$var[dlbl0;→ $var[d0]]$if[$var[dp0]==p]$var[dlbl0;→ $var[d0] ♛]$endif$addStringSelectOption[$var[dlbl0];mv~$var[fromSquare]~$var[d0]~$var[dp0];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;1]!=]$var[d1;$httpResult[moves;1]]$var[dp1;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d1];8]==true]$var[dp1;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d1];1]==true]$var[dp1;p]$endif$endif$var[dlbl1;→ $var[d1]]$if[$var[dp1]==p]$var[dlbl1;→ $var[d1] ♛]$endif$addStringSelectOption[$var[dlbl1];mv~$var[fromSquare]~$var[d1]~$var[dp1];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;2]!=]$var[d2;$httpResult[moves;2]]$var[dp2;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d2];8]==true]$var[dp2;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d2];1]==true]$var[dp2;p]$endif$endif$var[dlbl2;→ $var[d2]]$if[$var[dp2]==p]$var[dlbl2;→ $var[d2] ♛]$endif$addStringSelectOption[$var[dlbl2];mv~$var[fromSquare]~$var[d2]~$var[dp2];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;3]!=]$var[d3;$httpResult[moves;3]]$var[dp3;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d3];8]==true]$var[dp3;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d3];1]==true]$var[dp3;p]$endif$endif$var[dlbl3;→ $var[d3]]$if[$var[dp3]==p]$var[dlbl3;→ $var[d3] ♛]$endif$addStringSelectOption[$var[dlbl3];mv~$var[fromSquare]~$var[d3]~$var[dp3];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;4]!=]$var[d4;$httpResult[moves;4]]$var[dp4;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d4];8]==true]$var[dp4;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d4];1]==true]$var[dp4;p]$endif$endif$var[dlbl4;→ $var[d4]]$if[$var[dp4]==p]$var[dlbl4;→ $var[d4] ♛]$endif$addStringSelectOption[$var[dlbl4];mv~$var[fromSquare]~$var[d4]~$var[dp4];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;5]!=]$var[d5;$httpResult[moves;5]]$var[dp5;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d5];8]==true]$var[dp5;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d5];1]==true]$var[dp5;p]$endif$endif$var[dlbl5;→ $var[d5]]$if[$var[dp5]==p]$var[dlbl5;→ $var[d5] ♛]$endif$addStringSelectOption[$var[dlbl5];mv~$var[fromSquare]~$var[d5]~$var[dp5];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;6]!=]$var[d6;$httpResult[moves;6]]$var[dp6;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d6];8]==true]$var[dp6;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d6];1]==true]$var[dp6;p]$endif$endif$var[dlbl6;→ $var[d6]]$if[$var[dp6]==p]$var[dlbl6;→ $var[d6] ♛]$endif$addStringSelectOption[$var[dlbl6];mv~$var[fromSquare]~$var[d6]~$var[dp6];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;7]!=]$var[d7;$httpResult[moves;7]]$var[dp7;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d7];8]==true]$var[dp7;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d7];1]==true]$var[dp7;p]$endif$endif$var[dlbl7;→ $var[d7]]$if[$var[dp7]==p]$var[dlbl7;→ $var[d7] ♛]$endif$addStringSelectOption[$var[dlbl7];mv~$var[fromSquare]~$var[d7]~$var[dp7];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;8]!=]$var[d8;$httpResult[moves;8]]$var[dp8;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d8];8]==true]$var[dp8;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d8];1]==true]$var[dp8;p]$endif$endif$var[dlbl8;→ $var[d8]]$if[$var[dp8]==p]$var[dlbl8;→ $var[d8] ♛]$endif$addStringSelectOption[$var[dlbl8];mv~$var[fromSquare]~$var[d8]~$var[dp8];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;9]!=]$var[d9;$httpResult[moves;9]]$var[dp9;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d9];8]==true]$var[dp9;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d9];1]==true]$var[dp9;p]$endif$endif$var[dlbl9;→ $var[d9]]$if[$var[dp9]==p]$var[dlbl9;→ $var[d9] ♛]$endif$addStringSelectOption[$var[dlbl9];mv~$var[fromSquare]~$var[d9]~$var[dp9];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;10]!=]$var[d10;$httpResult[moves;10]]$var[dp10;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d10];8]==true]$var[dp10;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d10];1]==true]$var[dp10;p]$endif$endif$var[dlbl10;→ $var[d10]]$if[$var[dp10]==p]$var[dlbl10;→ $var[d10] ♛]$endif$addStringSelectOption[$var[dlbl10];mv~$var[fromSquare]~$var[d10]~$var[dp10];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;11]!=]$var[d11;$httpResult[moves;11]]$var[dp11;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d11];8]==true]$var[dp11;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d11];1]==true]$var[dp11;p]$endif$endif$var[dlbl11;→ $var[d11]]$if[$var[dp11]==p]$var[dlbl11;→ $var[d11] ♛]$endif$addStringSelectOption[$var[dlbl11];mv~$var[fromSquare]~$var[d11]~$var[dp11];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;12]!=]$var[d12;$httpResult[moves;12]]$var[dp12;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d12];8]==true]$var[dp12;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d12];1]==true]$var[dp12;p]$endif$endif$var[dlbl12;→ $var[d12]]$if[$var[dp12]==p]$var[dlbl12;→ $var[d12] ♛]$endif$addStringSelectOption[$var[dlbl12];mv~$var[fromSquare]~$var[d12]~$var[dp12];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;13]!=]$var[d13;$httpResult[moves;13]]$var[dp13;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d13];8]==true]$var[dp13;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d13];1]==true]$var[dp13;p]$endif$endif$var[dlbl13;→ $var[d13]]$if[$var[dp13]==p]$var[dlbl13;→ $var[d13] ♛]$endif$addStringSelectOption[$var[dlbl13];mv~$var[fromSquare]~$var[d13]~$var[dp13];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;14]!=]$var[d14;$httpResult[moves;14]]$var[dp14;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d14];8]==true]$var[dp14;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d14];1]==true]$var[dp14;p]$endif$endif$var[dlbl14;→ $var[d14]]$if[$var[dp14]==p]$var[dlbl14;→ $var[d14] ♛]$endif$addStringSelectOption[$var[dlbl14];mv~$var[fromSquare]~$var[d14]~$var[dp14];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;15]!=]$var[d15;$httpResult[moves;15]]$var[dp15;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d15];8]==true]$var[dp15;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d15];1]==true]$var[dp15;p]$endif$endif$var[dlbl15;→ $var[d15]]$if[$var[dp15]==p]$var[dlbl15;→ $var[d15] ♛]$endif$addStringSelectOption[$var[dlbl15];mv~$var[fromSquare]~$var[d15]~$var[dp15];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;16]!=]$var[d16;$httpResult[moves;16]]$var[dp16;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d16];8]==true]$var[dp16;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d16];1]==true]$var[dp16;p]$endif$endif$var[dlbl16;→ $var[d16]]$if[$var[dp16]==p]$var[dlbl16;→ $var[d16] ♛]$endif$addStringSelectOption[$var[dlbl16];mv~$var[fromSquare]~$var[d16]~$var[dp16];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;17]!=]$var[d17;$httpResult[moves;17]]$var[dp17;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d17];8]==true]$var[dp17;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d17];1]==true]$var[dp17;p]$endif$endif$var[dlbl17;→ $var[d17]]$if[$var[dp17]==p]$var[dlbl17;→ $var[d17] ♛]$endif$addStringSelectOption[$var[dlbl17];mv~$var[fromSquare]~$var[d17]~$var[dp17];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;18]!=]$var[d18;$httpResult[moves;18]]$var[dp18;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d18];8]==true]$var[dp18;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d18];1]==true]$var[dp18;p]$endif$endif$var[dlbl18;→ $var[d18]]$if[$var[dp18]==p]$var[dlbl18;→ $var[d18] ♛]$endif$addStringSelectOption[$var[dlbl18];mv~$var[fromSquare]~$var[d18]~$var[dp18];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;19]!=]$var[d19;$httpResult[moves;19]]$var[dp19;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d19];8]==true]$var[dp19;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d19];1]==true]$var[dp19;p]$endif$endif$var[dlbl19;→ $var[d19]]$if[$var[dp19]==p]$var[dlbl19;→ $var[d19] ♛]$endif$addStringSelectOption[$var[dlbl19];mv~$var[fromSquare]~$var[d19]~$var[dp19];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;20]!=]$var[d20;$httpResult[moves;20]]$var[dp20;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d20];8]==true]$var[dp20;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d20];1]==true]$var[dp20;p]$endif$endif$var[dlbl20;→ $var[d20]]$if[$var[dp20]==p]$var[dlbl20;→ $var[d20] ♛]$endif$addStringSelectOption[$var[dlbl20];mv~$var[fromSquare]~$var[d20]~$var[dp20];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;21]!=]$var[d21;$httpResult[moves;21]]$var[dp21;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d21];8]==true]$var[dp21;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d21];1]==true]$var[dp21;p]$endif$endif$var[dlbl21;→ $var[d21]]$if[$var[dp21]==p]$var[dlbl21;→ $var[d21] ♛]$endif$addStringSelectOption[$var[dlbl21];mv~$var[fromSquare]~$var[d21]~$var[dp21];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;22]!=]$var[d22;$httpResult[moves;22]]$var[dp22;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d22];8]==true]$var[dp22;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d22];1]==true]$var[dp22;p]$endif$endif$var[dlbl22;→ $var[d22]]$if[$var[dp22]==p]$var[dlbl22;→ $var[d22] ♛]$endif$addStringSelectOption[$var[dlbl22];mv~$var[fromSquare]~$var[d22]~$var[dp22];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;23]!=]$var[d23;$httpResult[moves;23]]$var[dp23;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d23];8]==true]$var[dp23;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d23];1]==true]$var[dp23;p]$endif$endif$var[dlbl23;→ $var[d23]]$if[$var[dp23]==p]$var[dlbl23;→ $var[d23] ♛]$endif$addStringSelectOption[$var[dlbl23];mv~$var[fromSquare]~$var[d23]~$var[dp23];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;24]!=]$var[d24;$httpResult[moves;24]]$var[dp24;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d24];8]==true]$var[dp24;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d24];1]==true]$var[dp24;p]$endif$endif$var[dlbl24;→ $var[d24]]$if[$var[dp24]==p]$var[dlbl24;→ $var[d24] ♛]$endif$addStringSelectOption[$var[dlbl24];mv~$var[fromSquare]~$var[d24]~$var[dp24];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chbp~$var[wID]~$var[gID];⬅️ Voltar;secondary;false;;ctrl]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Render-se;danger;false;;ctrl]
$stop
$endif

$if[$var[action]==chbpr]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[fromSquare;$splitText[4]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Você não tem partida ativa. Execute \`$var[trigger]\` para criar uma.]
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
$addTextDisplay[❌ Esta partida não está mais ativa. Execute \`$var[trigger]\` para criar uma nova.]
$stop
$endif

$if[$var[curStatus]==c]
$ephemeral
$addTextDisplay[ℹ️ O desafio ainda não foi aceito. Espere o oponente aceitar.]
$stop
$endif

$if[$var[curStatus]==do]
$ephemeral
$addTextDisplay[ℹ️ Há uma oferta de empate pendente. Responda-a primeiro.]
$stop
$endif

$if[$var[curStatus]!=p]
$ephemeral
$addTextDisplay[ℹ️ Esta partida já terminou. Use 🎲 Nova partida para jogar outra.]
$stop
$endif

$textSplit[$var[fen]; ]
$var[turn;$splitText[2]]
$var[expectedPlayer;$var[wID]]
$if[$var[turn]==b]$var[expectedPlayer;$var[bID]]$endif
$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ Você não é jogador desta partida.]
$stop
$endif
$endif
$if[$authorID!=$var[expectedPlayer]]
$ephemeral
$addTextDisplay[❌ Não é o seu turno. Espere o outro jogador jogar.]
$stop
$endif

$defer
$removeAllComponents
$httpGet[http://chess.nexusify.co/moves?fen=$url[encode;$var[fen]]&square=$var[fromSquare]]
$var[movesCount;$httpResult[count]]
$var[pieceLetter;$httpResult[piece]]
$var[pieceColor;$httpResult[piece_color]]
$var[turn;$var[pieceColor]]
$var[perspective;white]
$var[turnLabel;♔ Brancas]
$if[$var[turn]==b]
$var[turnLabel;♚ Pretas]
$endif
$var[lblP;♙ Peão]$var[lblN;♘ Cavalo]$var[lblB;♗ Bispo]$var[lblR;♖ Torre]$var[lblQ;♕ Dama]$var[lblK;♔ Rei]$if[$var[turn]==b]$var[lblP;♟ Peão]$var[lblN;♞ Cavalo]$var[lblB;♝ Bispo]$var[lblR;♜ Torre]$var[lblQ;♛ Dama]$var[lblK;♚ Rei]$endif
$var[pieceName;]
$if[$var[pieceLetter]==P]$var[pieceName;$var[lblP]]$endif
$if[$var[pieceLetter]==N]$var[pieceName;$var[lblN]]$endif
$if[$var[pieceLetter]==B]$var[pieceName;$var[lblB]]$endif
$if[$var[pieceLetter]==R]$var[pieceName;$var[lblR]]$endif
$if[$var[pieceLetter]==Q]$var[pieceName;$var[lblQ]]$endif
$if[$var[pieceLetter]==K]$var[pieceName;$var[lblK]]$endif
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;#5865F2;false]
$addTextDisplay[# ♟️ **Xadrez — Mover peça**

**Peça selecionada:** $var[pieceName] em \`$var[fromSquare]\`
**Turno:** $var[turnLabel];main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Tabuleiro de xadrez;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Para onde mover?** Selecione uma casa de destino:;main]
$addActionRow[r1;main]
$addStringSelect[chdest~$var[wID]~$var[gID]~$var[fromSquare];Selecionar destino...;1;1;;r1]
$if[$httpResult[moves;0]!=]$var[d0;$httpResult[moves;0]]$var[dp0;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d0];8]==true]$var[dp0;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d0];1]==true]$var[dp0;p]$endif$endif$var[dlbl0;→ $var[d0]]$if[$var[dp0]==p]$var[dlbl0;→ $var[d0] ♛]$endif$addStringSelectOption[$var[dlbl0];mv~$var[fromSquare]~$var[d0]~$var[dp0];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;1]!=]$var[d1;$httpResult[moves;1]]$var[dp1;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d1];8]==true]$var[dp1;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d1];1]==true]$var[dp1;p]$endif$endif$var[dlbl1;→ $var[d1]]$if[$var[dp1]==p]$var[dlbl1;→ $var[d1] ♛]$endif$addStringSelectOption[$var[dlbl1];mv~$var[fromSquare]~$var[d1]~$var[dp1];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;2]!=]$var[d2;$httpResult[moves;2]]$var[dp2;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d2];8]==true]$var[dp2;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d2];1]==true]$var[dp2;p]$endif$endif$var[dlbl2;→ $var[d2]]$if[$var[dp2]==p]$var[dlbl2;→ $var[d2] ♛]$endif$addStringSelectOption[$var[dlbl2];mv~$var[fromSquare]~$var[d2]~$var[dp2];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;3]!=]$var[d3;$httpResult[moves;3]]$var[dp3;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d3];8]==true]$var[dp3;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d3];1]==true]$var[dp3;p]$endif$endif$var[dlbl3;→ $var[d3]]$if[$var[dp3]==p]$var[dlbl3;→ $var[d3] ♛]$endif$addStringSelectOption[$var[dlbl3];mv~$var[fromSquare]~$var[d3]~$var[dp3];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;4]!=]$var[d4;$httpResult[moves;4]]$var[dp4;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d4];8]==true]$var[dp4;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d4];1]==true]$var[dp4;p]$endif$endif$var[dlbl4;→ $var[d4]]$if[$var[dp4]==p]$var[dlbl4;→ $var[d4] ♛]$endif$addStringSelectOption[$var[dlbl4];mv~$var[fromSquare]~$var[d4]~$var[dp4];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;5]!=]$var[d5;$httpResult[moves;5]]$var[dp5;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d5];8]==true]$var[dp5;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d5];1]==true]$var[dp5;p]$endif$endif$var[dlbl5;→ $var[d5]]$if[$var[dp5]==p]$var[dlbl5;→ $var[d5] ♛]$endif$addStringSelectOption[$var[dlbl5];mv~$var[fromSquare]~$var[d5]~$var[dp5];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;6]!=]$var[d6;$httpResult[moves;6]]$var[dp6;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d6];8]==true]$var[dp6;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d6];1]==true]$var[dp6;p]$endif$endif$var[dlbl6;→ $var[d6]]$if[$var[dp6]==p]$var[dlbl6;→ $var[d6] ♛]$endif$addStringSelectOption[$var[dlbl6];mv~$var[fromSquare]~$var[d6]~$var[dp6];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;7]!=]$var[d7;$httpResult[moves;7]]$var[dp7;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d7];8]==true]$var[dp7;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d7];1]==true]$var[dp7;p]$endif$endif$var[dlbl7;→ $var[d7]]$if[$var[dp7]==p]$var[dlbl7;→ $var[d7] ♛]$endif$addStringSelectOption[$var[dlbl7];mv~$var[fromSquare]~$var[d7]~$var[dp7];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;8]!=]$var[d8;$httpResult[moves;8]]$var[dp8;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d8];8]==true]$var[dp8;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d8];1]==true]$var[dp8;p]$endif$endif$var[dlbl8;→ $var[d8]]$if[$var[dp8]==p]$var[dlbl8;→ $var[d8] ♛]$endif$addStringSelectOption[$var[dlbl8];mv~$var[fromSquare]~$var[d8]~$var[dp8];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;9]!=]$var[d9;$httpResult[moves;9]]$var[dp9;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d9];8]==true]$var[dp9;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d9];1]==true]$var[dp9;p]$endif$endif$var[dlbl9;→ $var[d9]]$if[$var[dp9]==p]$var[dlbl9;→ $var[d9] ♛]$endif$addStringSelectOption[$var[dlbl9];mv~$var[fromSquare]~$var[d9]~$var[dp9];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;10]!=]$var[d10;$httpResult[moves;10]]$var[dp10;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d10];8]==true]$var[dp10;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d10];1]==true]$var[dp10;p]$endif$endif$var[dlbl10;→ $var[d10]]$if[$var[dp10]==p]$var[dlbl10;→ $var[d10] ♛]$endif$addStringSelectOption[$var[dlbl10];mv~$var[fromSquare]~$var[d10]~$var[dp10];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;11]!=]$var[d11;$httpResult[moves;11]]$var[dp11;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d11];8]==true]$var[dp11;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d11];1]==true]$var[dp11;p]$endif$endif$var[dlbl11;→ $var[d11]]$if[$var[dp11]==p]$var[dlbl11;→ $var[d11] ♛]$endif$addStringSelectOption[$var[dlbl11];mv~$var[fromSquare]~$var[d11]~$var[dp11];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;12]!=]$var[d12;$httpResult[moves;12]]$var[dp12;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d12];8]==true]$var[dp12;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d12];1]==true]$var[dp12;p]$endif$endif$var[dlbl12;→ $var[d12]]$if[$var[dp12]==p]$var[dlbl12;→ $var[d12] ♛]$endif$addStringSelectOption[$var[dlbl12];mv~$var[fromSquare]~$var[d12]~$var[dp12];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;13]!=]$var[d13;$httpResult[moves;13]]$var[dp13;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d13];8]==true]$var[dp13;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d13];1]==true]$var[dp13;p]$endif$endif$var[dlbl13;→ $var[d13]]$if[$var[dp13]==p]$var[dlbl13;→ $var[d13] ♛]$endif$addStringSelectOption[$var[dlbl13];mv~$var[fromSquare]~$var[d13]~$var[dp13];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;14]!=]$var[d14;$httpResult[moves;14]]$var[dp14;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d14];8]==true]$var[dp14;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d14];1]==true]$var[dp14;p]$endif$endif$var[dlbl14;→ $var[d14]]$if[$var[dp14]==p]$var[dlbl14;→ $var[d14] ♛]$endif$addStringSelectOption[$var[dlbl14];mv~$var[fromSquare]~$var[d14]~$var[dp14];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;15]!=]$var[d15;$httpResult[moves;15]]$var[dp15;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d15];8]==true]$var[dp15;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d15];1]==true]$var[dp15;p]$endif$endif$var[dlbl15;→ $var[d15]]$if[$var[dp15]==p]$var[dlbl15;→ $var[d15] ♛]$endif$addStringSelectOption[$var[dlbl15];mv~$var[fromSquare]~$var[d15]~$var[dp15];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;16]!=]$var[d16;$httpResult[moves;16]]$var[dp16;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d16];8]==true]$var[dp16;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d16];1]==true]$var[dp16;p]$endif$endif$var[dlbl16;→ $var[d16]]$if[$var[dp16]==p]$var[dlbl16;→ $var[d16] ♛]$endif$addStringSelectOption[$var[dlbl16];mv~$var[fromSquare]~$var[d16]~$var[dp16];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;17]!=]$var[d17;$httpResult[moves;17]]$var[dp17;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d17];8]==true]$var[dp17;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d17];1]==true]$var[dp17;p]$endif$endif$var[dlbl17;→ $var[d17]]$if[$var[dp17]==p]$var[dlbl17;→ $var[d17] ♛]$endif$addStringSelectOption[$var[dlbl17];mv~$var[fromSquare]~$var[d17]~$var[dp17];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;18]!=]$var[d18;$httpResult[moves;18]]$var[dp18;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d18];8]==true]$var[dp18;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d18];1]==true]$var[dp18;p]$endif$endif$var[dlbl18;→ $var[d18]]$if[$var[dp18]==p]$var[dlbl18;→ $var[d18] ♛]$endif$addStringSelectOption[$var[dlbl18];mv~$var[fromSquare]~$var[d18]~$var[dp18];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;19]!=]$var[d19;$httpResult[moves;19]]$var[dp19;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d19];8]==true]$var[dp19;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d19];1]==true]$var[dp19;p]$endif$endif$var[dlbl19;→ $var[d19]]$if[$var[dp19]==p]$var[dlbl19;→ $var[d19] ♛]$endif$addStringSelectOption[$var[dlbl19];mv~$var[fromSquare]~$var[d19]~$var[dp19];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;20]!=]$var[d20;$httpResult[moves;20]]$var[dp20;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d20];8]==true]$var[dp20;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d20];1]==true]$var[dp20;p]$endif$endif$var[dlbl20;→ $var[d20]]$if[$var[dp20]==p]$var[dlbl20;→ $var[d20] ♛]$endif$addStringSelectOption[$var[dlbl20];mv~$var[fromSquare]~$var[d20]~$var[dp20];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;21]!=]$var[d21;$httpResult[moves;21]]$var[dp21;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d21];8]==true]$var[dp21;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d21];1]==true]$var[dp21;p]$endif$endif$var[dlbl21;→ $var[d21]]$if[$var[dp21]==p]$var[dlbl21;→ $var[d21] ♛]$endif$addStringSelectOption[$var[dlbl21];mv~$var[fromSquare]~$var[d21]~$var[dp21];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;22]!=]$var[d22;$httpResult[moves;22]]$var[dp22;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d22];8]==true]$var[dp22;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d22];1]==true]$var[dp22;p]$endif$endif$var[dlbl22;→ $var[d22]]$if[$var[dp22]==p]$var[dlbl22;→ $var[d22] ♛]$endif$addStringSelectOption[$var[dlbl22];mv~$var[fromSquare]~$var[d22]~$var[dp22];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;23]!=]$var[d23;$httpResult[moves;23]]$var[dp23;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d23];8]==true]$var[dp23;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d23];1]==true]$var[dp23;p]$endif$endif$var[dlbl23;→ $var[d23]]$if[$var[dp23]==p]$var[dlbl23;→ $var[d23] ♛]$endif$addStringSelectOption[$var[dlbl23];mv~$var[fromSquare]~$var[d23]~$var[dp23];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$if[$httpResult[moves;24]!=]$var[d24;$httpResult[moves;24]]$var[dp24;-]$if[$var[pieceLetter]==P]$if[$checkContains[$var[d24];8]==true]$var[dp24;p]$endif$endif$if[$var[pieceLetter]==p]$if[$checkContains[$var[d24];1]==true]$var[dp24;p]$endif$endif$var[dlbl24;→ $var[d24]]$if[$var[dp24]==p]$var[dlbl24;→ $var[d24] ♛]$endif$addStringSelectOption[$var[dlbl24];mv~$var[fromSquare]~$var[d24]~$var[dp24];Casa;;;chdest~$var[wID]~$var[gID]~$var[fromSquare]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chbp~$var[wID]~$var[gID];⬅️ Voltar;secondary;false;;ctrl]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Render-se;danger;false;;ctrl]
$stop
$endif

$if[$var[action]==chpr]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[fromSquare;$splitText[4]]
$var[toSquare;$splitText[5]]
$var[promoPiece;$message]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Você não tem partida ativa. Execute \`$var[trigger]\` para criar uma.]
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
$addTextDisplay[❌ Esta partida não está mais ativa. Execute \`$var[trigger]\` para criar uma nova.]
$stop
$endif

$if[$var[curStatus]==c]
$ephemeral
$addTextDisplay[ℹ️ O desafio ainda não foi aceito. Espere o oponente aceitar.]
$stop
$endif

$if[$var[curStatus]==do]
$ephemeral
$addTextDisplay[ℹ️ Há uma oferta de empate pendente. Responda-a primeiro.]
$stop
$endif

$if[$var[curStatus]!=p]
$ephemeral
$addTextDisplay[ℹ️ Esta partida já terminou. Use 🎲 Nova partida para jogar outra.]
$stop
$endif

$textSplit[$var[fen]; ]
$var[turn;$splitText[2]]
$var[expectedPlayer;$var[wID]]
$if[$var[turn]==b]$var[expectedPlayer;$var[bID]]$endif
$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ Você não é jogador desta partida.]
$stop
$endif
$endif
$if[$authorID!=$var[expectedPlayer]]
$ephemeral
$addTextDisplay[❌ Não é o seu turno. Espere o outro jogador jogar.]
$stop
$endif

$defer
$var[mvFrom;$var[fromSquare]]
$var[mvTo;$var[toSquare]]
$httpGet[http://chess.nexusify.co/apply?fen=$url[encode;$var[fen]]&from=$var[mvFrom]&to=$var[mvTo]&promotion=$var[promoPiece]]
$var[legal;$httpResult[legal]]
$if[$var[legal]!=true]
$ephemeral
$addTextDisplay[❌ Lance ilegal: $httpResult[error]]
$stop
$endif

$var[newFen;$httpResult[fen]]
$var[newStatus;$httpResult[status]]
$var[newSAN;$httpResult[move_san]]
$var[isCheckmate;$httpResult[is_checkmate]]
$var[isStalemate;$httpResult[is_stalemate]]
$var[isDraw;$httpResult[is_draw]]

$var[finalStatus;p]
$if[$var[isCheckmate]==true]
$var[finalStatus;$var[turn]]
$endif
$if[$var[isStalemate]==true]
$var[finalStatus;d]
$endif
$if[$var[isDraw]==true]
$var[finalStatus;d]
$endif

$var[fen;$var[newFen]]
$var[lastMoveSAN;$var[newSAN]]

$var[newState;$var[newFen]|$var[wID]|$var[bID]|$var[gID]|$var[finalStatus]|$var[mvFrom]$var[mvTo]|$var[newSAN]|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$if[$var[finalStatus]!=p]
$removeAllComponents
$var[cc;#ED4245]
$var[resultMsg;🚪 Partida abandonada]
$if[$var[finalStatus]==w]
$var[cc;#57F287]
$var[resultMsg;🏆 Brancas vencem! <@$var[wID]>]
$endif
$if[$var[finalStatus]==b]
$var[cc;#57F287]
$var[resultMsg;🏆 Pretas vencem! <@$var[bID]>]
$endif
$if[$var[finalStatus]==d]
$var[cc;#FEE75C]
$var[resultMsg;🤝 Empate! Partida empatada]
$endif
$var[perspective;white]
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Xadrez — Partida finalizada**

$var[resultMsg]

**Último lance:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Tabuleiro final;false;board]
$addSeparator[true;small;main]
$addTextDisplay[📊 **Resultado:** $var[resultMsg];main]
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chnew~$var[wID]~$var[bID];🎲 Nova partida;success;false;;ctrl]
$addButtonCV2[chclose~$var[wID]~$var[gID];🗑️ Fechar;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Jogadores:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$removeAllComponents
$httpGet[http://chess.nexusify.co/moves?fen=$url[encode;$var[fen]]]
$var[movesCount;$httpResult[count]]
$var[turn;$httpResult[turn]]
$var[cc;#5865F2]
$var[turnLabel;♔ Brancas]
$var[activePlayer;$var[wID]]
$var[perspective;white]
$if[$var[turn]==b]
$var[turnLabel;♚ Pretas]
$var[activePlayer;$var[bID]]
$endif
$var[lblP;♙ Peão]$var[lblN;♘ Cavalo]$var[lblB;♗ Bispo]$var[lblR;♖ Torre]$var[lblQ;♕ Dama]$var[lblK;♔ Rei]$if[$var[turn]==b]$var[lblP;♟ Peão]$var[lblN;♞ Cavalo]$var[lblB;♝ Bispo]$var[lblR;♜ Torre]$var[lblQ;♛ Dama]$var[lblK;♚ Rei]$endif
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Xadrez**

**Turno:** $var[turnLabel]
**A jogar:** <@$var[activePlayer]>
**Último lance:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Tabuleiro de xadrez;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Seu turno, <@$var[activePlayer]>.** Selecione uma peça para mover:;main]
$addActionRow[r1;main]
$addStringSelect[chpiece~$var[wID]~$var[gID];Selecionar peça...;1;1;;r1]
$if[$httpResult[moves_by_square;0;from]!=]$var[p0;$httpResult[moves_by_square;0;piece]]$var[from0;$httpResult[moves_by_square;0;from]]$var[lbl0;$var[lblP]]$if[$var[p0]==N]$var[lbl0;$var[lblN]]$endif$if[$var[p0]==B]$var[lbl0;$var[lblB]]$endif$if[$var[p0]==R]$var[lbl0;$var[lblR]]$endif$if[$var[p0]==Q]$var[lbl0;$var[lblQ]]$endif$if[$var[p0]==K]$var[lbl0;$var[lblK]]$endif$addStringSelectOption[$var[lbl0] em $var[from0];$var[from0];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;1;from]!=]$var[p1;$httpResult[moves_by_square;1;piece]]$var[from1;$httpResult[moves_by_square;1;from]]$var[lbl1;$var[lblP]]$if[$var[p1]==N]$var[lbl1;$var[lblN]]$endif$if[$var[p1]==B]$var[lbl1;$var[lblB]]$endif$if[$var[p1]==R]$var[lbl1;$var[lblR]]$endif$if[$var[p1]==Q]$var[lbl1;$var[lblQ]]$endif$if[$var[p1]==K]$var[lbl1;$var[lblK]]$endif$addStringSelectOption[$var[lbl1] em $var[from1];$var[from1];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;2;from]!=]$var[p2;$httpResult[moves_by_square;2;piece]]$var[from2;$httpResult[moves_by_square;2;from]]$var[lbl2;$var[lblP]]$if[$var[p2]==N]$var[lbl2;$var[lblN]]$endif$if[$var[p2]==B]$var[lbl2;$var[lblB]]$endif$if[$var[p2]==R]$var[lbl2;$var[lblR]]$endif$if[$var[p2]==Q]$var[lbl2;$var[lblQ]]$endif$if[$var[p2]==K]$var[lbl2;$var[lblK]]$endif$addStringSelectOption[$var[lbl2] em $var[from2];$var[from2];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;3;from]!=]$var[p3;$httpResult[moves_by_square;3;piece]]$var[from3;$httpResult[moves_by_square;3;from]]$var[lbl3;$var[lblP]]$if[$var[p3]==N]$var[lbl3;$var[lblN]]$endif$if[$var[p3]==B]$var[lbl3;$var[lblB]]$endif$if[$var[p3]==R]$var[lbl3;$var[lblR]]$endif$if[$var[p3]==Q]$var[lbl3;$var[lblQ]]$endif$if[$var[p3]==K]$var[lbl3;$var[lblK]]$endif$addStringSelectOption[$var[lbl3] em $var[from3];$var[from3];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;4;from]!=]$var[p4;$httpResult[moves_by_square;4;piece]]$var[from4;$httpResult[moves_by_square;4;from]]$var[lbl4;$var[lblP]]$if[$var[p4]==N]$var[lbl4;$var[lblN]]$endif$if[$var[p4]==B]$var[lbl4;$var[lblB]]$endif$if[$var[p4]==R]$var[lbl4;$var[lblR]]$endif$if[$var[p4]==Q]$var[lbl4;$var[lblQ]]$endif$if[$var[p4]==K]$var[lbl4;$var[lblK]]$endif$addStringSelectOption[$var[lbl4] em $var[from4];$var[from4];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;5;from]!=]$var[p5;$httpResult[moves_by_square;5;piece]]$var[from5;$httpResult[moves_by_square;5;from]]$var[lbl5;$var[lblP]]$if[$var[p5]==N]$var[lbl5;$var[lblN]]$endif$if[$var[p5]==B]$var[lbl5;$var[lblB]]$endif$if[$var[p5]==R]$var[lbl5;$var[lblR]]$endif$if[$var[p5]==Q]$var[lbl5;$var[lblQ]]$endif$if[$var[p5]==K]$var[lbl5;$var[lblK]]$endif$addStringSelectOption[$var[lbl5] em $var[from5];$var[from5];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;6;from]!=]$var[p6;$httpResult[moves_by_square;6;piece]]$var[from6;$httpResult[moves_by_square;6;from]]$var[lbl6;$var[lblP]]$if[$var[p6]==N]$var[lbl6;$var[lblN]]$endif$if[$var[p6]==B]$var[lbl6;$var[lblB]]$endif$if[$var[p6]==R]$var[lbl6;$var[lblR]]$endif$if[$var[p6]==Q]$var[lbl6;$var[lblQ]]$endif$if[$var[p6]==K]$var[lbl6;$var[lblK]]$endif$addStringSelectOption[$var[lbl6] em $var[from6];$var[from6];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;7;from]!=]$var[p7;$httpResult[moves_by_square;7;piece]]$var[from7;$httpResult[moves_by_square;7;from]]$var[lbl7;$var[lblP]]$if[$var[p7]==N]$var[lbl7;$var[lblN]]$endif$if[$var[p7]==B]$var[lbl7;$var[lblB]]$endif$if[$var[p7]==R]$var[lbl7;$var[lblR]]$endif$if[$var[p7]==Q]$var[lbl7;$var[lblQ]]$endif$if[$var[p7]==K]$var[lbl7;$var[lblK]]$endif$addStringSelectOption[$var[lbl7] em $var[from7];$var[from7];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;8;from]!=]$var[p8;$httpResult[moves_by_square;8;piece]]$var[from8;$httpResult[moves_by_square;8;from]]$var[lbl8;$var[lblP]]$if[$var[p8]==N]$var[lbl8;$var[lblN]]$endif$if[$var[p8]==B]$var[lbl8;$var[lblB]]$endif$if[$var[p8]==R]$var[lbl8;$var[lblR]]$endif$if[$var[p8]==Q]$var[lbl8;$var[lblQ]]$endif$if[$var[p8]==K]$var[lbl8;$var[lblK]]$endif$addStringSelectOption[$var[lbl8] em $var[from8];$var[from8];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;9;from]!=]$var[p9;$httpResult[moves_by_square;9;piece]]$var[from9;$httpResult[moves_by_square;9;from]]$var[lbl9;$var[lblP]]$if[$var[p9]==N]$var[lbl9;$var[lblN]]$endif$if[$var[p9]==B]$var[lbl9;$var[lblB]]$endif$if[$var[p9]==R]$var[lbl9;$var[lblR]]$endif$if[$var[p9]==Q]$var[lbl9;$var[lblQ]]$endif$if[$var[p9]==K]$var[lbl9;$var[lblK]]$endif$addStringSelectOption[$var[lbl9] em $var[from9];$var[from9];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;10;from]!=]$var[p10;$httpResult[moves_by_square;10;piece]]$var[from10;$httpResult[moves_by_square;10;from]]$var[lbl10;$var[lblP]]$if[$var[p10]==N]$var[lbl10;$var[lblN]]$endif$if[$var[p10]==B]$var[lbl10;$var[lblB]]$endif$if[$var[p10]==R]$var[lbl10;$var[lblR]]$endif$if[$var[p10]==Q]$var[lbl10;$var[lblQ]]$endif$if[$var[p10]==K]$var[lbl10;$var[lblK]]$endif$addStringSelectOption[$var[lbl10] em $var[from10];$var[from10];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;11;from]!=]$var[p11;$httpResult[moves_by_square;11;piece]]$var[from11;$httpResult[moves_by_square;11;from]]$var[lbl11;$var[lblP]]$if[$var[p11]==N]$var[lbl11;$var[lblN]]$endif$if[$var[p11]==B]$var[lbl11;$var[lblB]]$endif$if[$var[p11]==R]$var[lbl11;$var[lblR]]$endif$if[$var[p11]==Q]$var[lbl11;$var[lblQ]]$endif$if[$var[p11]==K]$var[lbl11;$var[lblK]]$endif$addStringSelectOption[$var[lbl11] em $var[from11];$var[from11];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;12;from]!=]$var[p12;$httpResult[moves_by_square;12;piece]]$var[from12;$httpResult[moves_by_square;12;from]]$var[lbl12;$var[lblP]]$if[$var[p12]==N]$var[lbl12;$var[lblN]]$endif$if[$var[p12]==B]$var[lbl12;$var[lblB]]$endif$if[$var[p12]==R]$var[lbl12;$var[lblR]]$endif$if[$var[p12]==Q]$var[lbl12;$var[lblQ]]$endif$if[$var[p12]==K]$var[lbl12;$var[lblK]]$endif$addStringSelectOption[$var[lbl12] em $var[from12];$var[from12];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;13;from]!=]$var[p13;$httpResult[moves_by_square;13;piece]]$var[from13;$httpResult[moves_by_square;13;from]]$var[lbl13;$var[lblP]]$if[$var[p13]==N]$var[lbl13;$var[lblN]]$endif$if[$var[p13]==B]$var[lbl13;$var[lblB]]$endif$if[$var[p13]==R]$var[lbl13;$var[lblR]]$endif$if[$var[p13]==Q]$var[lbl13;$var[lblQ]]$endif$if[$var[p13]==K]$var[lbl13;$var[lblK]]$endif$addStringSelectOption[$var[lbl13] em $var[from13];$var[from13];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;14;from]!=]$var[p14;$httpResult[moves_by_square;14;piece]]$var[from14;$httpResult[moves_by_square;14;from]]$var[lbl14;$var[lblP]]$if[$var[p14]==N]$var[lbl14;$var[lblN]]$endif$if[$var[p14]==B]$var[lbl14;$var[lblB]]$endif$if[$var[p14]==R]$var[lbl14;$var[lblR]]$endif$if[$var[p14]==Q]$var[lbl14;$var[lblQ]]$endif$if[$var[p14]==K]$var[lbl14;$var[lblK]]$endif$addStringSelectOption[$var[lbl14] em $var[from14];$var[from14];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;15;from]!=]$var[p15;$httpResult[moves_by_square;15;piece]]$var[from15;$httpResult[moves_by_square;15;from]]$var[lbl15;$var[lblP]]$if[$var[p15]==N]$var[lbl15;$var[lblN]]$endif$if[$var[p15]==B]$var[lbl15;$var[lblB]]$endif$if[$var[p15]==R]$var[lbl15;$var[lblR]]$endif$if[$var[p15]==Q]$var[lbl15;$var[lblQ]]$endif$if[$var[p15]==K]$var[lbl15;$var[lblK]]$endif$addStringSelectOption[$var[lbl15] em $var[from15];$var[from15];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Render-se;danger;false;;ctrl]
$addButtonCV2[chdraw~$var[wID]~$var[gID];🤝 Oferecer empate;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Jogadores:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$if[$var[action]==chdest]
$var[wID;$splitText[2]]
$var[gID;$splitText[3]]
$var[fromSquare;$splitText[4]]
$var[selValue;$message]

$textSplit[$var[selValue];~]
$var[mvFrom;$splitText[2]]
$var[mvTo;$splitText[3]]
$var[mvPromo;$splitText[4]]

$var[state;$getVar[chess_state;$var[wID]]]
$if[$var[state]==]
$ephemeral
$addTextDisplay[❌ Você não tem partida ativa. Execute \`$var[trigger]\` para criar uma.]
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
$addTextDisplay[❌ Esta partida não está mais ativa. Execute \`$var[trigger]\` para criar uma nova.]
$stop
$endif

$if[$var[curStatus]==c]
$ephemeral
$addTextDisplay[ℹ️ O desafio ainda não foi aceito. Espere o oponente aceitar.]
$stop
$endif

$if[$var[curStatus]==do]
$ephemeral
$addTextDisplay[ℹ️ Há uma oferta de empate pendente. Responda-a primeiro.]
$stop
$endif

$if[$var[curStatus]!=p]
$ephemeral
$addTextDisplay[ℹ️ Esta partida já terminou. Use 🎲 Nova partida para jogar outra.]
$stop
$endif

$textSplit[$var[fen]; ]
$var[turn;$splitText[2]]
$var[expectedPlayer;$var[wID]]
$if[$var[turn]==b]$var[expectedPlayer;$var[bID]]$endif
$if[$authorID!=$var[wID]]
$if[$authorID!=$var[bID]]
$ephemeral
$addTextDisplay[❌ Você não é jogador desta partida.]
$stop
$endif
$endif
$if[$authorID!=$var[expectedPlayer]]
$ephemeral
$addTextDisplay[❌ Não é o seu turno. Espere o outro jogador jogar.]
$stop
$endif

$defer
$if[$var[mvPromo]==p]
$var[toSquare;$var[mvTo]]
$removeAllComponents
$var[perspective;white]
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;#FEE75C;false]
$addTextDisplay[# ♟️ **Xadrez — Promoção**

Seu peão em \`$var[fromSquare]\` alcançou a última fileira. Escolha a peça para promover:;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Tabuleiro de xadrez;false;board]
$addSeparator[true;small;main]
$addActionRow[r1;main]
$addStringSelect[chpr~$var[wID]~$var[gID]~$var[fromSquare]~$var[toSquare];Escolha a peça de promoção...;1;1;;r1]
$addStringSelectOption[♕ Dama;q;Promover a dama (★ recomendado);♕;;chpr~$var[wID]~$var[gID]~$var[fromSquare]~$var[toSquare]]
$addStringSelectOption[♖ Torre;r;Promover a torre;;;chpr~$var[wID]~$var[gID]~$var[fromSquare]~$var[toSquare]]
$addStringSelectOption[♗ Bispo;b;Promover a bispo;;;chpr~$var[wID]~$var[gID]~$var[fromSquare]~$var[toSquare]]
$addStringSelectOption[♘ Cavalo;n;Promover a cavalo;;;chpr~$var[wID]~$var[gID]~$var[fromSquare]~$var[toSquare]]
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chbpr~$var[wID]~$var[gID]~$var[fromSquare];⬅️ Voltar;secondary;false;;ctrl]
$stop
$endif

$var[promoPiece;q]
$httpGet[http://chess.nexusify.co/apply?fen=$url[encode;$var[fen]]&from=$var[mvFrom]&to=$var[mvTo]&promotion=$var[promoPiece]]
$var[legal;$httpResult[legal]]
$if[$var[legal]!=true]
$ephemeral
$addTextDisplay[❌ Lance ilegal: $httpResult[error]]
$stop
$endif

$var[newFen;$httpResult[fen]]
$var[newStatus;$httpResult[status]]
$var[newSAN;$httpResult[move_san]]
$var[isCheckmate;$httpResult[is_checkmate]]
$var[isStalemate;$httpResult[is_stalemate]]
$var[isDraw;$httpResult[is_draw]]

$var[finalStatus;p]
$if[$var[isCheckmate]==true]
$var[finalStatus;$var[turn]]
$endif
$if[$var[isStalemate]==true]
$var[finalStatus;d]
$endif
$if[$var[isDraw]==true]
$var[finalStatus;d]
$endif

$var[fen;$var[newFen]]
$var[lastMoveSAN;$var[newSAN]]

$var[newState;$var[newFen]|$var[wID]|$var[bID]|$var[gID]|$var[finalStatus]|$var[mvFrom]$var[mvTo]|$var[newSAN]|$var[theme]|$var[trigger]]
$setVar[chess_state;$var[newState];$var[wID]]

$if[$var[finalStatus]!=p]
$removeAllComponents
$var[cc;#ED4245]
$var[resultMsg;🚪 Partida abandonada]
$if[$var[finalStatus]==w]
$var[cc;#57F287]
$var[resultMsg;🏆 Brancas vencem! <@$var[wID]>]
$endif
$if[$var[finalStatus]==b]
$var[cc;#57F287]
$var[resultMsg;🏆 Pretas vencem! <@$var[bID]>]
$endif
$if[$var[finalStatus]==d]
$var[cc;#FEE75C]
$var[resultMsg;🤝 Empate! Partida empatada]
$endif
$var[perspective;white]
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Xadrez — Partida finalizada**

$var[resultMsg]

**Último lance:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Tabuleiro final;false;board]
$addSeparator[true;small;main]
$addTextDisplay[📊 **Resultado:** $var[resultMsg];main]
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chnew~$var[wID]~$var[bID];🎲 Nova partida;success;false;;ctrl]
$addButtonCV2[chclose~$var[wID]~$var[gID];🗑️ Fechar;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Jogadores:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$removeAllComponents
$httpGet[http://chess.nexusify.co/moves?fen=$url[encode;$var[fen]]]
$var[movesCount;$httpResult[count]]
$var[turn;$httpResult[turn]]
$var[cc;#5865F2]
$var[turnLabel;♔ Brancas]
$var[activePlayer;$var[wID]]
$var[perspective;white]
$if[$var[turn]==b]
$var[turnLabel;♚ Pretas]
$var[activePlayer;$var[bID]]
$endif
$var[lblP;♙ Peão]$var[lblN;♘ Cavalo]$var[lblB;♗ Bispo]$var[lblR;♖ Torre]$var[lblQ;♕ Dama]$var[lblK;♔ Rei]$if[$var[turn]==b]$var[lblP;♟ Peão]$var[lblN;♞ Cavalo]$var[lblB;♝ Bispo]$var[lblR;♜ Torre]$var[lblQ;♛ Dama]$var[lblK;♚ Rei]$endif
$var[boardUrl;http://chess.nexusify.co/board.png?fen=$url[encode;$var[fen]]&perspective=$var[perspective]&size=600&theme=$var[theme]]
$addContainer[main;$var[cc];false]
$addTextDisplay[# ♟️ **Xadrez**

**Turno:** $var[turnLabel]
**A jogar:** <@$var[activePlayer]>
**Último lance:** \`$var[lastMoveSAN]\`;main]
$addSeparator[true;small;main]
$addMediaGallery[board;main]
$addMediaGalleryItem[$var[boardUrl];Tabuleiro de xadrez;false;board]
$addSeparator[true;small;main]
$addTextDisplay[🎯 **Seu turno, <@$var[activePlayer]>.** Selecione uma peça para mover:;main]
$addActionRow[r1;main]
$addStringSelect[chpiece~$var[wID]~$var[gID];Selecionar peça...;1;1;;r1]
$if[$httpResult[moves_by_square;0;from]!=]$var[p0;$httpResult[moves_by_square;0;piece]]$var[from0;$httpResult[moves_by_square;0;from]]$var[lbl0;$var[lblP]]$if[$var[p0]==N]$var[lbl0;$var[lblN]]$endif$if[$var[p0]==B]$var[lbl0;$var[lblB]]$endif$if[$var[p0]==R]$var[lbl0;$var[lblR]]$endif$if[$var[p0]==Q]$var[lbl0;$var[lblQ]]$endif$if[$var[p0]==K]$var[lbl0;$var[lblK]]$endif$addStringSelectOption[$var[lbl0] em $var[from0];$var[from0];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;1;from]!=]$var[p1;$httpResult[moves_by_square;1;piece]]$var[from1;$httpResult[moves_by_square;1;from]]$var[lbl1;$var[lblP]]$if[$var[p1]==N]$var[lbl1;$var[lblN]]$endif$if[$var[p1]==B]$var[lbl1;$var[lblB]]$endif$if[$var[p1]==R]$var[lbl1;$var[lblR]]$endif$if[$var[p1]==Q]$var[lbl1;$var[lblQ]]$endif$if[$var[p1]==K]$var[lbl1;$var[lblK]]$endif$addStringSelectOption[$var[lbl1] em $var[from1];$var[from1];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;2;from]!=]$var[p2;$httpResult[moves_by_square;2;piece]]$var[from2;$httpResult[moves_by_square;2;from]]$var[lbl2;$var[lblP]]$if[$var[p2]==N]$var[lbl2;$var[lblN]]$endif$if[$var[p2]==B]$var[lbl2;$var[lblB]]$endif$if[$var[p2]==R]$var[lbl2;$var[lblR]]$endif$if[$var[p2]==Q]$var[lbl2;$var[lblQ]]$endif$if[$var[p2]==K]$var[lbl2;$var[lblK]]$endif$addStringSelectOption[$var[lbl2] em $var[from2];$var[from2];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;3;from]!=]$var[p3;$httpResult[moves_by_square;3;piece]]$var[from3;$httpResult[moves_by_square;3;from]]$var[lbl3;$var[lblP]]$if[$var[p3]==N]$var[lbl3;$var[lblN]]$endif$if[$var[p3]==B]$var[lbl3;$var[lblB]]$endif$if[$var[p3]==R]$var[lbl3;$var[lblR]]$endif$if[$var[p3]==Q]$var[lbl3;$var[lblQ]]$endif$if[$var[p3]==K]$var[lbl3;$var[lblK]]$endif$addStringSelectOption[$var[lbl3] em $var[from3];$var[from3];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;4;from]!=]$var[p4;$httpResult[moves_by_square;4;piece]]$var[from4;$httpResult[moves_by_square;4;from]]$var[lbl4;$var[lblP]]$if[$var[p4]==N]$var[lbl4;$var[lblN]]$endif$if[$var[p4]==B]$var[lbl4;$var[lblB]]$endif$if[$var[p4]==R]$var[lbl4;$var[lblR]]$endif$if[$var[p4]==Q]$var[lbl4;$var[lblQ]]$endif$if[$var[p4]==K]$var[lbl4;$var[lblK]]$endif$addStringSelectOption[$var[lbl4] em $var[from4];$var[from4];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;5;from]!=]$var[p5;$httpResult[moves_by_square;5;piece]]$var[from5;$httpResult[moves_by_square;5;from]]$var[lbl5;$var[lblP]]$if[$var[p5]==N]$var[lbl5;$var[lblN]]$endif$if[$var[p5]==B]$var[lbl5;$var[lblB]]$endif$if[$var[p5]==R]$var[lbl5;$var[lblR]]$endif$if[$var[p5]==Q]$var[lbl5;$var[lblQ]]$endif$if[$var[p5]==K]$var[lbl5;$var[lblK]]$endif$addStringSelectOption[$var[lbl5] em $var[from5];$var[from5];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;6;from]!=]$var[p6;$httpResult[moves_by_square;6;piece]]$var[from6;$httpResult[moves_by_square;6;from]]$var[lbl6;$var[lblP]]$if[$var[p6]==N]$var[lbl6;$var[lblN]]$endif$if[$var[p6]==B]$var[lbl6;$var[lblB]]$endif$if[$var[p6]==R]$var[lbl6;$var[lblR]]$endif$if[$var[p6]==Q]$var[lbl6;$var[lblQ]]$endif$if[$var[p6]==K]$var[lbl6;$var[lblK]]$endif$addStringSelectOption[$var[lbl6] em $var[from6];$var[from6];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;7;from]!=]$var[p7;$httpResult[moves_by_square;7;piece]]$var[from7;$httpResult[moves_by_square;7;from]]$var[lbl7;$var[lblP]]$if[$var[p7]==N]$var[lbl7;$var[lblN]]$endif$if[$var[p7]==B]$var[lbl7;$var[lblB]]$endif$if[$var[p7]==R]$var[lbl7;$var[lblR]]$endif$if[$var[p7]==Q]$var[lbl7;$var[lblQ]]$endif$if[$var[p7]==K]$var[lbl7;$var[lblK]]$endif$addStringSelectOption[$var[lbl7] em $var[from7];$var[from7];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;8;from]!=]$var[p8;$httpResult[moves_by_square;8;piece]]$var[from8;$httpResult[moves_by_square;8;from]]$var[lbl8;$var[lblP]]$if[$var[p8]==N]$var[lbl8;$var[lblN]]$endif$if[$var[p8]==B]$var[lbl8;$var[lblB]]$endif$if[$var[p8]==R]$var[lbl8;$var[lblR]]$endif$if[$var[p8]==Q]$var[lbl8;$var[lblQ]]$endif$if[$var[p8]==K]$var[lbl8;$var[lblK]]$endif$addStringSelectOption[$var[lbl8] em $var[from8];$var[from8];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;9;from]!=]$var[p9;$httpResult[moves_by_square;9;piece]]$var[from9;$httpResult[moves_by_square;9;from]]$var[lbl9;$var[lblP]]$if[$var[p9]==N]$var[lbl9;$var[lblN]]$endif$if[$var[p9]==B]$var[lbl9;$var[lblB]]$endif$if[$var[p9]==R]$var[lbl9;$var[lblR]]$endif$if[$var[p9]==Q]$var[lbl9;$var[lblQ]]$endif$if[$var[p9]==K]$var[lbl9;$var[lblK]]$endif$addStringSelectOption[$var[lbl9] em $var[from9];$var[from9];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;10;from]!=]$var[p10;$httpResult[moves_by_square;10;piece]]$var[from10;$httpResult[moves_by_square;10;from]]$var[lbl10;$var[lblP]]$if[$var[p10]==N]$var[lbl10;$var[lblN]]$endif$if[$var[p10]==B]$var[lbl10;$var[lblB]]$endif$if[$var[p10]==R]$var[lbl10;$var[lblR]]$endif$if[$var[p10]==Q]$var[lbl10;$var[lblQ]]$endif$if[$var[p10]==K]$var[lbl10;$var[lblK]]$endif$addStringSelectOption[$var[lbl10] em $var[from10];$var[from10];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;11;from]!=]$var[p11;$httpResult[moves_by_square;11;piece]]$var[from11;$httpResult[moves_by_square;11;from]]$var[lbl11;$var[lblP]]$if[$var[p11]==N]$var[lbl11;$var[lblN]]$endif$if[$var[p11]==B]$var[lbl11;$var[lblB]]$endif$if[$var[p11]==R]$var[lbl11;$var[lblR]]$endif$if[$var[p11]==Q]$var[lbl11;$var[lblQ]]$endif$if[$var[p11]==K]$var[lbl11;$var[lblK]]$endif$addStringSelectOption[$var[lbl11] em $var[from11];$var[from11];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;12;from]!=]$var[p12;$httpResult[moves_by_square;12;piece]]$var[from12;$httpResult[moves_by_square;12;from]]$var[lbl12;$var[lblP]]$if[$var[p12]==N]$var[lbl12;$var[lblN]]$endif$if[$var[p12]==B]$var[lbl12;$var[lblB]]$endif$if[$var[p12]==R]$var[lbl12;$var[lblR]]$endif$if[$var[p12]==Q]$var[lbl12;$var[lblQ]]$endif$if[$var[p12]==K]$var[lbl12;$var[lblK]]$endif$addStringSelectOption[$var[lbl12] em $var[from12];$var[from12];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;13;from]!=]$var[p13;$httpResult[moves_by_square;13;piece]]$var[from13;$httpResult[moves_by_square;13;from]]$var[lbl13;$var[lblP]]$if[$var[p13]==N]$var[lbl13;$var[lblN]]$endif$if[$var[p13]==B]$var[lbl13;$var[lblB]]$endif$if[$var[p13]==R]$var[lbl13;$var[lblR]]$endif$if[$var[p13]==Q]$var[lbl13;$var[lblQ]]$endif$if[$var[p13]==K]$var[lbl13;$var[lblK]]$endif$addStringSelectOption[$var[lbl13] em $var[from13];$var[from13];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;14;from]!=]$var[p14;$httpResult[moves_by_square;14;piece]]$var[from14;$httpResult[moves_by_square;14;from]]$var[lbl14;$var[lblP]]$if[$var[p14]==N]$var[lbl14;$var[lblN]]$endif$if[$var[p14]==B]$var[lbl14;$var[lblB]]$endif$if[$var[p14]==R]$var[lbl14;$var[lblR]]$endif$if[$var[p14]==Q]$var[lbl14;$var[lblQ]]$endif$if[$var[p14]==K]$var[lbl14;$var[lblK]]$endif$addStringSelectOption[$var[lbl14] em $var[from14];$var[from14];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$if[$httpResult[moves_by_square;15;from]!=]$var[p15;$httpResult[moves_by_square;15;piece]]$var[from15;$httpResult[moves_by_square;15;from]]$var[lbl15;$var[lblP]]$if[$var[p15]==N]$var[lbl15;$var[lblN]]$endif$if[$var[p15]==B]$var[lbl15;$var[lblB]]$endif$if[$var[p15]==R]$var[lbl15;$var[lblR]]$endif$if[$var[p15]==Q]$var[lbl15;$var[lblQ]]$endif$if[$var[p15]==K]$var[lbl15;$var[lblK]]$endif$addStringSelectOption[$var[lbl15] em $var[from15];$var[from15];Peça;;;chpiece~$var[wID]~$var[gID]]$endif
$addSeparator[true;small;main]
$addActionRow[ctrl;main]
$addButtonCV2[chforfeit~$var[wID]~$var[gID];🏳️ Render-se;danger;false;;ctrl]
$addButtonCV2[chdraw~$var[wID]~$var[gID];🤝 Oferecer empate;secondary;false;;ctrl]
$addSeparator[true;small;main]
$addTextDisplay[**Jogadores:** <@$var[wID]> (♔) vs <@$var[bID]> (♚) • **ID:** \`$var[gID]\`;main]
$stop
$endif

$catch
$ephemeral
$addTextDisplay[❌ Ocorreu um erro inesperado: $error[message]]
$endtry
```

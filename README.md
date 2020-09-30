PROJECT NAME : CONNECT FOUR.

INTRODUCTION:
My project is about a game where we have 7 rows and 6
columns where at a time two players where these two players
need to select one colour either red or blue and if 4 in a
horizontal,vertical or diagonal meet the player with that
colour wins the game.
Let’s see how the game start :
STARTING OF THE GAME WE GET:

STEPS OF HOW THE GAME WORKS:

Step-1: In the beginning of the game it starts with the red
colour and then blue colour it goes from the bottom to top.

Step-2: After that step-1 we can start the game like one after
the other first player gets the chance and then the second

player gets the chance like the game continues till any colour
match once it matches then it wins.

Step-3: Now if 4 colours in horizontal,vertical or diagonal
meet the player with that colour wins the game.

Step 4: As the game if you want to play again you can find a
restart button at the bottom of the screen we can start from the
beginning.

Now lets us go through the code HTML,CSS,JAVASCRIPT.

index.html

&lt;html&gt;
&lt;head&gt;
&lt;title&gt;Connect 4&lt;/title&gt;
&lt;link rel=&quot;stylesheet&quot; href=&quot;style.css&quot;&gt;
&lt;script src=&quot;https://cdnjs.cloudflare.com/ajax/libs/jquery/3.2.1/jquery.min.js&quot;&gt;&lt;/script&gt;
&lt;script src=&quot;connect4.js&quot;&gt;&lt;/script&gt;
&lt;script src=&quot;main.js&quot;&gt;&lt;/script&gt;
&lt;/head&gt;
&lt;body&gt;
&lt;style&gt;
body {
background-image: url(&#39;image.jpg&#39;);
background-repeat: no-repeat;
background-attachment: fixed;
background-size: 100% 100%;
}
&lt;/style&gt;
&lt;div class=&quot;container&quot; align=&quot;center&quot;&gt;
&lt;h1 style=&quot;color:white;&quot;&gt;Welcome to connect four! &lt;/h1&gt;
&lt;h2 style=&quot;color:white;&quot;&gt;Connect 4 chips to win &lt;/h2&gt;
&lt;h3 style=&quot;color:white;&quot;&gt;Let&#39;s start!&lt;/h3&gt;
&lt;body&gt;
&lt;h4&gt;It&#39;s &lt;span id=&quot;player&quot;&gt;red&lt;/span&gt;&#39;s turn!&lt;/h4&gt;
&lt;div id=&quot;connect4&quot;&gt;&lt;/div&gt;
&lt;br&gt;
&lt;button id=&quot;restart&quot;&gt;Restart&lt;/button&gt;
&lt;/body&gt;
&lt;/html&gt;

style.css

body {
text-align: center;
}

#connect4 {
display: inline-block;
}
.col {
width: 70px;
height: 70px;
display: inline-block;
background-color: gray;
border: 4px solid black;
border-radius: 50%;
margin: 1px;
}
.col.empty {
cursor: pointer;
}
.next-red {
background-color: rgba(255, 0, 0, 0.2);
}
.next-blue {
background-color: rgba(0, 181, 204, 1);
}
.col.red {
background-color: red;
}
.col.blue {
background-color: blue;
}

main.js

$(document).ready(function() {
const connect4 = new Connect4(&#39;#connect4&#39;)
connect4.onPlayerMove = function() {
$(&#39;#player&#39;).text(connect4.player);

}
$(&#39;#restart&#39;).click(function() {
connect4.restart();
})
});

connect4.js

class Connect4 {
constructor(selector) {
this.ROWS = 6;
this.COLS = 7;
this.player = &#39;red&#39;;
this.selector = selector;
this.isGameOver = false;
this.onPlayerMove = function() {};
this.createGrid();
this.setupEventListeners();
}
createGrid() {
const $board = $(this.selector);
$board.empty();
this.isGameOver = false;
this.player = &#39;red&#39;;
for (let row = 0; row &lt; this.ROWS; row++) {
const $row = $(&#39;&lt;div&gt;&#39;)
.addClass(&#39;row&#39;);
for (let col = 0; col &lt; this.COLS; col++) {
const $col = $(&#39;&lt;div&gt;&#39;)
.addClass(&#39;col empty&#39;)
.attr(&#39;data-col&#39;, col)
.attr(&#39;data-row&#39;, row);
$row.append($col);
}
$board.append($row);
}
}
setupEventListeners() {
const $board = $(this.selector);
const that = this;
function findLastEmptyCell(col) {
const cells = $(`.col[data-col=&#39;${col}&#39;]`);

for (let i = cells.length - 1; i &gt;= 0; i--) {
const $cell = $(cells[i]);
if ($cell.hasClass(&#39;empty&#39;)) {
return $cell;
}
}
return null;
}
$board.on(&#39;mouseenter&#39;, &#39;.col.empty&#39;, function() {
if (that.isGameOver) return;
const col = $(this).data(&#39;col&#39;);
const $lastEmptyCell = findLastEmptyCell(col);
$lastEmptyCell.addClass(`next-${that.player}`);
});
$board.on(&#39;mouseleave&#39;, &#39;.col&#39;, function() {
$(&#39;.col&#39;).removeClass(`next-${that.player}`);
});
$board.on(&#39;click&#39;, &#39;.col.empty&#39;, function() {
if (that.isGameOver) return;
const col = $(this).data(&#39;col&#39;);
const $lastEmptyCell = findLastEmptyCell(col);
$lastEmptyCell.removeClass(`empty next-${that.player}`);
$lastEmptyCell.addClass(that.player);
$lastEmptyCell.data(&#39;player&#39;, that.player);
const winner = that.checkForWinner(
$lastEmptyCell.data(&#39;row&#39;),
$lastEmptyCell.data(&#39;col&#39;)
)
if (winner) {
that.isGameOver = true;
alert(`Game Over! Player ${that.player} has won!`);
$(&#39;.col.empty&#39;).removeClass(&#39;empty&#39;);
return;
}
that.player = (that.player === &#39;red&#39;) ? &#39;blue&#39; : &#39;red&#39;;
that.onPlayerMove();
$(this).trigger(&#39;mouseenter&#39;);
});
}
checkForWinner(row, col) {
const that = this;

function $getCell(i, j) {
return $(`.col[data-row=&#39;${i}&#39;][data-col=&#39;${j}&#39;]`);
}
function checkDirection(direction) {
let total = 0;
let i = row + direction.i;
let j = col + direction.j;
let $next = $getCell(i, j);
while (i &gt;= 0 &amp;&amp;
i &lt; that.ROWS &amp;&amp;
j &gt;= 0 &amp;&amp;
j &lt; that.COLS &amp;&amp;
$next.data(&#39;player&#39;) === that.player
) {
total++;
i += direction.i;
j += direction.j;
$next = $getCell(i, j);
}
return total;
}
function checkWin(directionA, directionB) {
const total = 1 +
checkDirection(directionA) +
checkDirection(directionB);
if (total &gt;= 4) {
return that.player;
} else {
return null;
}
}
function checkDiagonalBLtoTR() {
return checkWin({i: 1, j: -1}, {i: 1, j: 1})
}
function checkDiagonalTLtoBR() {
return checkWin({i: 1, j: 1}, {i: -1, j: -1});
}
function checkVerticals() {
return checkWin({i: -1, j: 0}, {i: 1, j: 0});
}
function checkHorizontals() {
return checkWin({i: 0, j: -1}, {i: 0, j: 1});

}
return checkVerticals() ||
checkHorizontals() ||
checkDiagonalBLtoTR() ||
checkDiagonalTLtoBR();
}
restart () {
this.createGrid();
this.onPlayerMove();
}
}

image.jpg

THE LINK FOR WHOLE CODES WAS GIVEN BELOW

https://drive.google.com/drive/folders/1Td80BgDytpyYFDXmx173GFvxREC_uL0b?usp=sharing

CONCLUSION:
Games such as Connect Four, and the project was manageable given the tools we’ve
accrued throughout the semester. Understanding patch graphics, matrices, ginput,
for loops, and if/else statements was essential for creating this game. Debugging was
necessary throughout the coding process, but often the problems we encountered
required only logic to understand how our code had room for misinterpretation, and
with trial we made it hermetic. We could further improve the game by replacing

redundant code for each turn with a function, and creating a method for clearing the
board and playing again by button “Restart” without re-running the script. However,
the skills we gained in graphics and loops could support further work in creating
games and perhaps for making visual models of processes relating to science and
engineering.

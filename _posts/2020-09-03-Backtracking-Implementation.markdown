---
layout: post
title:  "Solving Puzzles With Backtracking Algorithm."
description: In this post, I am going to show you how I implemented the backtracking algorithm to solve a Sudoku board as well as arrive at the right solution of a Peg Solitaire board.
date:   2020-09-10
categories: Algorithms
comments: False
---
<!-- Put introduction stuff -->
<!-- <h2><b>A little bit about backtracking.</b></h2> -->
<p>
    Backtracking is a technique used for problem solving where the algorithm follows a brute-force approach. Unlike Dynamic programming, which looks for the optimal solution, backtracking is used when there are multiple solutions to the problem and all of them need to be found.
</p>
<p> 
    Backtracking uses a recursive calling function to find a solution in a step-by-step manner. Each problem has a unique set of constraints which are checked after every step. If the currently found solution holds good with the constraints, then the program continues to the next stage. Otherwise, the program performs an undo operation where the last update is undone, and the next feasible solution for the is found. 
</p>
<p>
    So, backtracking algorithm can be seen as trying to find a path to the solution with small barriers where the problem can backtrack if there is no feasible solution for the problem.
</p>

<h2><b>Solving a Sudoku Board. </b></h2>
<p>
    The regular Sudoku board is made up of a partially filled 9 x 9 grid of cells. To find the solution, one must fill each empty cell with a number between 1 and 9 such that the row, its respective column and subgrid contains exactly one instance of the digit.
</p>

<h3><b>Approach: Brute Force vs. Backtracking</b></h3>
<p>
     A general approach to solving the puzzle would be to employ a Brute Force algorithm. This algorithm would be responsible for generating all the board, regardless of whether they are correct or no. This means that the algorithm is tasked with a task of generating a large number of digits for each board and this results in a combinational explosion as the size of the board increases.
</p>
<p>
    Instead of this, I employed the technique of backtracking to solve the problem. The algorithm selects an empty cell and generates a number for this cell. It then proceeds to check whether this number belongs to the cell and doesn't break the board. If the number is proven to be in the right position, the algorithm continues by looking for the next empty cell to fill. If the number is proven to be wrong for the position, the algorithm removes this number and tries the next digit. Hence, it performs backtracking and results in much lesser computation than the general brute force approach.
</p>

<h3><b>Writing Code!</b></h3>

<h4><b>Finding An Empty Cell</b><br/>
<script src="https://gist.github.com/agk98/943d47510bbbe9fa77223f4da0472e69.js"></script>
<p>
    The 9 x 9 Sudoku board is stored as 2-dimensional list. The empty cells are filled with the number '0' while the rest of the cells are filled with their numbers.
</p>
<p>
    The algorithm starts by finding an empty cell to fill. It does this by looping through cells in every row and column. When the cell has the value of 0, it is said to be empty and the function <b>find_empty</b> returns the respective co-ordinates in the form of a tuple with format <b>(row,column)</b>. 
</p>

<h4><b>Check Validity </b></h4>
<script src="https://gist.github.com/agk98/0fbd7c67e575b0e36151ac88b21c226a.js"></script>
<p>
    Once an empty cell is found, it is to be filled with a number between 1 and 9. The number should fit all the constraints of the problem and to check for it's validity, the function <b>check_validity</b> performs a series of checks.<br>
    It first checks to make sure that the inserted <b>number</b> is unique to that that respective row and column.<br>
    After this, the sub-grid that contains the current cell is checked. This is a 3 x 3 grid and must contain unique numbers between 1 and 9. To get the starting corner of the sub-grid, the row and column positions are divided by 3 and the floor value of this is retrieved. They are divided by 3 as the board is seen as a 3 x 3 grid of sub-grids. With the starting position of the sub-grid, the rows and columns of that subgrid are tested for validity of the numbers.<br>
    Only if the <b>number</b> passes all the constraints, then the function returns <b>True</b>. Otherwise, it returns <b>False</b>, prompting the algorithm to test the next number.
</p>

<h4><b>Solving Board </b></h4>
<script src="https://gist.github.com/agk98/263eaf6333a0003af99ace397abd11da.js"></script>
<p>
    This is the entry point of the program. After finding an empty cell, it finds a valid number to fit that cell. After assigning the valid number to that cell, it continues to find the next empty cell by recursively calling the function. <br>Everytime that the function finds out that the new number breaks the board, it backtracks and looks for a new solution.<br>Finally, once the solution is found, the board can be printed. In this way, backtracking can be used to find the solution to a Sudoku Board.
</p>


<h2><b>Solving a Peg Solitaire Board. </b></h2>

<h3><b>The Game</b></h3>
<p>
    Peg Solitaire is a board game involving a single player moving pegs on a board with holes. It is famously called as Brainvita in India. There are many variants of the board, but this post focuses on solving the English Solitaire Board.
</p>
<p>
    To start, the board is entirely filled with pegs, except for the central hole. The objective of the game is to move the pegs around and ending up with the central hole being the only one occupied with a peg. The game has a few valid moves which need to be followed to achieve the goal. A valid move is to jump a peg either <b>up</b>,<b>down</b>,<b>left</b> or <b>right</b> only. The peg is made to jump over an adjacent peg into a hole two positions away and then remove the peg that it jumped over of. 
</p>

<h3><b>The working of the solver.</b></h3>
<p>
    The main task in solving the board would be to find the next move and make sure that it is legal. Incase of a bad move, the algorithm should be able to undo the most recent moves, essentially, backtracking.
</p>
<h4><b>Finding the next move </b></h4>
<script src="https://gist.github.com/agk98/254ce585d7931cc8567ea1af3e2f1926.js"></script>
<p>
    The everytime that a move is made, the program is able to know how many pegs are left on the board. Since the total number of pegs that can be on the board are 32, the value of <b>self.pegs</b> constantly updated as moves are made and undone.
    The legal directions are stored in the list <b>self.directions</b> and the this is traversed when searching for the next legal move.
</p>
<p>
    While searching for the next move, the algorithm considers a matrix of dimension 3x2, <b>cur_move</b>. Based on the direction of movement, the row or column of that matrix is updated by calling <b>update_move</b>. In this, the corrent peg's position is always pointed at by <b>cur_move[0][0]</b>. Based on the current direction, the changes that occur to the matrix:
    <ol>
        <li><b>"up"</b> or <b>"down"</b>
        <ul>
            <li> The <b>row</b> is updated as the peg is moving in a vertical direction and the <b>col</b> remains the same.</li>
        </ul></li>
        <li><b>"right"</b> or <b>"left"</b>
        <ul>
            <li> The <b>col</b> is updated as the peg is moving in a horizontal direction and the <b>row</b> remains the same.</li>
        </ul></li>
    </ol> 
</p>

<h4><b>Updating Board and checking for Legality</b></h4>
<script src="https://gist.github.com/agk98/e2ea207948d1f846dc6449b0ac3bb558.js"></script>
<p>
    With each move that is made, the value of <b>self.pegs</b> is decremented as the move results in the removal of a peg form the board. When the legality of this move is found to be bad, the move is reversed by calling <b>undo_move</b> which returns the board to its previous state and also increments <b>self.pegs</b> to account for this change. 
</p>


<h2><b>Thank You!</b></h2>
<p>
    With this, I was able to implement backtracking technique to solve these puzzles easily. This shows that the technique is very efficient in solving techniques and substituting it for some scenraios where brute-force would take way too long. This concludes the post. As usual, the entire code to both implementations can be found on my Github repository.
</p>
<div style="text-align:center">
    <a class="no_underline" href="https://github.com/agk98/Puzzle_solvers" target="_blank"><input type="button" value="Puzzle Solver Source Code"></a>
</div>

<h2><b>References</b></h2>
<p>
    <b><a href="https://www.youtube.com/watch?v=eqUwSA0xI-s" target="_blank">Tech With Tim</a><br/>
    <a href="http://courses.csail.mit.edu/6.884/spring10/labs/lab5.pdf" target="_blank">Backtracking to solve Peg Solitaire.</a><br/>
    <a href="https://github.com/arnaskro/Peg-Solitaire" target="_blank">Peg Solitaire Solver In Java</a><br/></b>
</p>

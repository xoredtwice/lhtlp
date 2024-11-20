# lhtlp
Linear homomorphic time-lock puzzle implementation in Mathematica

The implementation is based on the construction #5 (Figure 7) of [Cicada, A framework for private non-interactive on-chain auctions and voting(2024)](https://eprint.iacr.org/2023/1473.pdf) .
## Script Parameters
The run-script is at the end of the file in "TLP Test" section. You can set the hyperParamters to change the test: 
+ ```T``` : number of iterations to solve the puzzle.
+ ```sMax```: max value of the solution. This paramter has been used to construct the lookUp table.
+ ```qRange```: range of primes (p and q) to construct N = p * q.
+ ```solution1```: solution to puzzle1
+ ```solution2```: solution to puzzle2

## Functions
### ```pp, lookupTable = setup[T, qRange, sMax]```
+ outputs
  + publicParamters ```pp = <|N, g, h, y, T, sMax|> ```,
  + lookupTable ```{y^i mod N, i<sMax}```
### ```puzzle = generatePuzzle[pp, solution, r]```
+ outputs ```puzzle = <| "c1" = c1, "c2" = c2|>``` where
  + ```c1 = (pp.g ^ r) mod N```  and
  + ```c2 = (pp.h ^ r * pp.y ^ solution) mod N```
### ```ys = solvePuzzle[pp, puzzle]```
+ outputs ```ys = (pp.y ^ solution) mod N```

### ```puzzle3 = addPuzzles[pp, puzzle1, puzzle2]```
+ outputs  ```puzzle = <| "c1" = c1, "c2" = c2|>``` where
  +  ```c1 = (puzzle1.c1 * puzzle2.c1) mod N```,
  +  ```c2 = (puzzle1.c2 * puzzle2.c2) mod N```

### How it works
+ first setup would be called to generate public paramters based on hyper-paramteres.
+ Then it creates two independant puzzles ```puzzle1```, ```puzzle2``` and solves them.
+ Next, it adds two puzzles and generates the third puzzle ```puzzle3``` and solves this one too.
+ Then, using the lookupTable, it finds the decoded solutions to the puzzles.
+ Finally it asserts the equality of the decoded solution of ```puzzle3``` and summation of solutions 1 and 2. 

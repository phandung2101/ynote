> Also known as "Brute force approach". Use when we want to know all possibilities not the optimized ways.

Backtracking is like trying to find different path, when you hit a dead end, you backtrack the last choice and try a different route. It's like a method for finding the right way through a complex choices.
# What is backtrack?
- problem-solving algorithmic technique that involves finding a solution incrementally by trying **different options** and **undoing** them if they lead to a **dead end**
- use when you want to explore multiple possibilties to solve a problems (like searching a path for a maze or solving puzzles like Sudoku). When a dead end is reached, the algo backtrack to previous desicion point and explore another path until a solution is found or all 
# Basic Terminologies
- ***Candidate:*** A candidate is a potential choice or element that can be added to the current solution.
- ***Solution:*** The solution is a valid and complete configuration that satisfies all problem constraints.
- ***Partial Solution:*** A partial solution is an intermediate or incomplete configuration being constructed during the backtracking process.
- ***Decision Space:*** The decision space is the set of all possible candidates or choices at each decision point.
- ***Decision Point:*** A decision point is a specific step in the algorithm where a candidate is chosen and added to the partial solution.
- ***Feasible Solution:*** A feasible solution is a partial or complete solution that adheres to all constraints.
- ***Dead End:*** A dead end occurs when a partial solution cannot be extended without violating constraints.
- ***Backtrack:*** Backtracking involves undoing previous decisions and returning to a prior decision point.
- **Search Space:** The search space includes all possible combinations of candidates and choices.
- ***Optimal Solution:*** In optimization problems, the optimal solution is the best possible solution.
# How to apply?
- **Understand the Problem**:
    - Define the goal (e.g., place N queens).
    - Identify constraints (e.g., no queens attack).
    - Represent the state (e.g., board with queen positions).
- **Set Up Choices**:
    - Break problem into steps (e.g., choose column per row).
    - Model as a tree of possible choices.
- **Create Backtracking Function**:
    - Write a recursive function backtrack(state).
    - Check if state is a complete solution.
    - Try valid choices, update state, recurse, undo.
- **Check Complete Solution**:
    - If state meets goal (e.g., all queens placed), save/output it.
- **List Valid Choices**:
    - For current step, find allowed options (e.g., safe columns).
    - Skip invalid ones using constraints.
- **Apply and Backtrack**:
    - For each valid choice:
        - Update state (e.g., place queen).
        - Recurse to next step.
        - Undo choice to try others.
- **Optimize (Optional)**:
    - Skip dead-end branches early (e.g., invalid partial solutions).
    - Prioritize promising choices.
- **Handle End Cases**:
    - If no valid choices left and incomplete, return.
    - Stop when all solutions found or explored.
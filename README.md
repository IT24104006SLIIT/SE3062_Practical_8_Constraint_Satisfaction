SE3062 – Practical 8: Constraint Satisfaction Problems (CSP)

Module: SE3062 – Introduction to Artificial Intelligence| SLIIT Student: Jananjana J.W.C (IT24104006) | Lecturer: Jeewaka Perera

A from-scratch Python implementation of CSP solving techniques, built in a Jupyter notebook. The solvers are tested on Australia map coloring and benchmarked on 8-Queens.

What this project covers
Task	Topic	What was implemented
1	Naive Backtracking	Depth-first search with commutativity and consistency checks, with assignment and backtrack counters
2	Search Heuristics	MRV (fewest remaining values), Degree tie-breaker, LCV (least constraining value)
3	Constraint Propagation	AC-3 arc consistency (revise and ripple effect), used inside MAC (Maintaining Arc Consistency)
4	Ablation Study	Benchmark of all three strategies on 8-Queens (assignments, backtracks, time)
Repository contents

.
├── SE3062_Practical_8_Constraint_Satisfaction.ipynb   # main notebook (code, outputs, analysis)

└── README.md


How to run

Requirements: Python 3.8+ and the following packages:
```bash
pip install jupyter matplotlib networkx

```
Run the notebook:
```bash
jupyter notebook SE3062_Practical_8_Constraint_Satisfaction.ipynb

Then choose Kernel → Restart & Run All. Every task has a test block with assertions. A successful run prints >>> ALL ASSERTIONS PASSED FOR TASK N. for Tasks 1 to 3.
```

Implementation notes
Backtracking (Task 1): assigns one variable at a time, checks consistency with already-assigned neighbors, and undoes an assignment (backtracks) when the recursive call fails.
MRV + Degree (Task 2): variables are sorted by (remaining legal values, -unassigned neighbors), so the most constrained variable is chosen first and the Degree heuristic breaks ties. On the empty Australia map this picks SA first.
LCV (Task 2): for each legal value, counts how many options it removes from unassigned neighbors, then tries the least damaging value first.
AC-3 (Task 3): for each arc (Xi, Xj), removes any value of Xi that has no supporting value in Xj. When Xi shrinks, the arcs (Xk, Xi) are re-queued, and an empty domain signals failure immediately.
MAC (Task 3): backtracking combined with AC-3 after every assignment.

Results: 8-Queens ablation
Strategy	Assignments	Backtracks	Time (s)
Naive Backtracking	113	105	0.0018
MRV + Degree + LCV	89	81	0.0030
MAC (AC-3 + MRV)	20	12	0.0016

Observations
Naive backtracking made 113 assignments, and 105 were undone. It uses a fixed variable order and only detects conflicts after committing to them.
MRV + Degree + LCV reduced assignments by about 21%. The heuristics guide the search but cannot see dead ends coming. Per-node overhead made it slightly slower in wall-clock time on a problem this small.
MAC needed about 82% fewer assignments than naive search. AC-3 prunes impossible values after every placement and abandons a branch as soon as any domain becomes empty.

Takeaway: use inference (logic) to avoid searching, and heuristics to guide the search when guessing is unavoidable. These counts come from a single run, and the timings are only milliseconds, so assignments and backtracks are the reliable comparison.

Acknowledgements
Lecture material and the practical template are from SE3062 Lecture 11 and Practical 8. The lecture is adapted from the UIUC CS440 course materials on Constraint Satisfaction Problems by Svetlana Lazebnik and Mark Hasegawa-Johnson.

This repository is for academic coursework.

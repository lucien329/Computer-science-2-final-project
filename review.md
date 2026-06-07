# Review — BEN KRAM Mohamed Khalil & BOULAY Lucien — Project: Île-de-France route planner

## Fit with the topic
- Excellent fit. The whole brief is covered: GTFS parsing, graph construction, Dijkstra + A*, visualization.
- Modular architecture (parser / graph / solver / viz / cli) is clean and well-designed.
- The (station, line) node decomposition with transfer edges is the **right** modeling, it cleanly absorbs interchange costs into the graph rather than special-casing them in the solver. Excellent design choice, and exactly the kind of reasoning the course asks for.
- Honoring `transfers.txt` and the hub-node trick for boarding/alighting: very mature design.

## Ambition level
- **Very strong**. Make sure you have not over-engineered things before having a basic Dijkstra running on a tiny graph.
- **Recommendation**: get an end-to-end MVP working on a 10-station hand-built graph **first**, then layer the GTFS pipeline. Don't spend two weeks parsing before you've seen Dijkstra return a path.

## Algorithmic depth
- Be able to **explain** at the oral:
  - properties of algorithms, e.g. the priority-queue invariant of Dijkstra
  - what "admissible" and "consistent" mean for an A* heuristic, and why you actually need them,
  - why `great_circle_distance / max_speed` is admissible (and what `max_speed` must be in your transfer-aware graph).
- Your A* heuristic over a graph with transfer penalties is subtle: if your heuristic only models riding time, it remains admissible (transfers only add cost), but be explicit about this in the report.

## Open questions you listed
- **Effect of transfer penalty on chosen routes**: great experimental question. Pick a small set of representative OD pairs (long, short, multi-transfer) and sweep the penalty. A clean figure where the route "flips" is nice for the report.
- **Best `max_speed` for the heuristic**: be careful with what "best" means. A larger `max_speed` → weaker (but still admissible) heuristic → more nodes expanded. A smaller `max_speed` → tighter heuristic but risk of becoming **inadmissible** (and losing optimality). Frame the experiment as a trade-off, not an optimization.
- When A* beats Dijkstra: classic result, expect a strong correlation with the source–target Euclidean distance. Plot nodes-expanded ratio against great-circle distance.

## Extension ideas
- **Bidirectional Dijkstra** (you mentioned it): worth implementing, gives a noticeable speedup on long routes. Watch the stopping criterion, it is **not** "the two searches meet"; the correct condition is more subtle (see CLRS / standard references).
- **Time-expanded graph**: you mention it for topological sort. Don't necessarily implement, but at least discuss: with real timetable data, edge weights depend on departure time, and the route at 7am isn't the same as at 11pm.
- **Theoretical guarantees**: state and motivate the `O((V+E) log V)` bound for Dijkstra with binary heap and discuss what happens with Fibonacci heaps (no need to implement).
- **Practical measures**: nodes expanded, peak memory, wall time, queue length over time. A profile of "queue size vs. iteration" makes a nice figure.
- **MST as sanity check**: you mentioned it: yes, do it, it is a nice visual.

## AI warning
Your proposal is already very polished. To be honest, the writing style raises a small flag for me. To dispel any doubt:

- The **parser** (GTFS handling, edge construction, transfer modeling) must be your own work. AI can help with pandas idioms, not with the modeling. 
- **Dijkstra and A*** must be hand-written. They are the algorithmic core of the project and of the entire S2 chapter on shortest paths. You must each be able to write Dijkstra at the board, from scratch, if asked.
- The benchmark scripts and plotting can be AI-assisted (cite it).
- The report (analyses, comparisons, discussions of the experimental questions) must be your own.

**At the oral, we can ask you to walk through your Dijkstra implementation line by line and to modify it (e.g., return the second-best path).**

⚡ Bolt: Optimize N+1 GitHub API calls in duel loop

💡 **What:**
Lifted the `get_miner_generations` call for the current `leader` out of the challengers loop in `_run_duels`. When a challenger wins, the cached `leader_gens` is updated with the `challenger_gens`.

🎯 **Why:**
The `_run_duels` loop fetches generation data from the GitHub API using `get_miner_generations`. Previously, it was fetching both the leader's and the challenger's generations on *every* loop iteration. Because a leader can remain the same across multiple duels (successfully defending against challengers), re-fetching the same JSON file for the leader on every loop iteration wastes time and increases latency.

📊 **Impact:**
Reduces the number of network calls to the GitHub API for generation data from $O(2N)$ to $O(N+1)$ where N is the number of challengers evaluated. This will visibly speed up the judge service's DUEL stage.

🔬 **Measurement:**
Review execution time and network calls (or GitHub rate limit metrics) during the DUELS stage.

```python
#Randomly generate an initial feasible solution.
#For subsequent search, prioritize installing items into the backpack based on their cost-effectiveness, from lowest to highest.
#random solution
def random_feasible_init(values, weights, constraints, num_knapsacks, rng=None):
    rng = np.random.default_rng() if rng is None else rng
    N, D = weights.shape; K = num_knapsacks
    sol = np.zeros((N, K), dtype=int)
    loads = np.zeros((K, D), dtype=int)

    # from low to high value/weight ratio
    norm = np.maximum(constraints.astype(float), 1.0)
    score = values / ((weights / norm).sum(axis=1) + 1e-9)
    order = np.argsort(score)                   

    for i in order:
        k = rng.integers(K)                     
        if np.all(loads[k] + weights[i] <= constraints):
            sol[i, k] = 1
            loads[k] += weights[i]
    return sol
```
________________________________________________________________
```python
def fitness(sol, values):
    # only return the fitness value
    return int((sol * values[:, None]).sum())

def knapsack_loads(sol, weights):
    #Matrix multiplication
    return sol.T @ weights  # (K, D)
```
________________________________________________________________

```python
#Add first, then swap to generate neighbors
# 1. add i to some bag k
    for i in range(N):
        if sol[i].any():
            continue
        wi = weights[i]
        for k in range(K):
            if np.all(loads[k] + wi <= constraints):
                new_sol = sol.copy()
                new_sol[i, :] = 0
                new_sol[i, k] = 1
                gain = int(fitness(new_sol, values) - cur_fit)
                if gain > 0:
                    return new_sol, gain, {"op": "add", "i": i, "j": None, "k_from": None, "k_to": k}

    # 2. swap： swap i to bag k's j
    for k in range(K):
        for j in range(N):
            if sol[j, k] != 1:
                continue
            for i in range(N):
                if sol[i].any():
                    continue
                if np.all(loads[k] - weights[j] + weights[i] <= constraints):
                    new_sol = sol.copy()
                    new_sol[j, :] = 0
                    new_sol[i, :] = 0
                    new_sol[i, k] = 1
                    gain = int(fitness(new_sol, values) - cur_fit)
                    if gain > 0:
                        return new_sol, gain, {"op": "swap", "i": i, "j": j, "k_from": k, "k_to": k}

```

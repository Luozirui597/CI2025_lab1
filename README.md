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

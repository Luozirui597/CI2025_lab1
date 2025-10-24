# Review

When starting to use more dependencies (numpy, icecream, tqdm, ipywidgets) it's useful to have a `pyrproject.toml` file so that it's easy to check what needs to be installed.

Code wise I think that the implementation is good, I like a lot the way `random_feasible_init` works, with the ordering. The 3rd problem seems unusually slow though, it could be that for this simple problem a "dumber" but faster algorithm might perform better.
# *WIP* Polynomial Calculus Ruleset

The polynomial calculus proof-system encodes problems whose solutions are equivalent to proving that a family of polynomials has no common zero. As an example, consider the family `{XY-1, X}`. To prove that this family has no common zeros, we can take linear combinations or multiply a polynomial by a variable in order to find a reduction to the constant polynomial `1`. One such proof looks like:

```
[1] XY = X * Y           | multiplication by Y
[2] 1  = (XY) - (XY - 1) | linear combination
```

To encode this proof in a ruleset, we use ...

TODO: fill this in

*NB*: Typically, we constrain to binary variable values so if we ever get `X^2 - X` we reduce it to `0`.


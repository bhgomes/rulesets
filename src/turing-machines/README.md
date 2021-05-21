# Turing Machines

Rulesets can represent arbitrary (multi-tape) non-deterministic turing machines.

## Unbounded Tape

In order to simulate the "unbounded tape" feature of turing machines, we need a way to represent the tape data structure which can be arbitrarily extended. One way to encode it is via association lists with three elements like the following example:

```
(((-INF -2) -1) HEAD (1 (2 (3 (4 INF)))))
```

where `HEAD` represents the current position of the head and `-INF` and `INF` are special (constant) terminal symbols which denote the left and right ends of the tape respectively. Structurally, we have the left half of the tape, the current head symbol, and the right half of the tape, typically denoted by `l`, `_`, and `r` as follows:

```
(l _ r)
```

To extend the tape we can use the following two rules:

**LEFT-EXTENSION**
```
((
    (-INF _ r)
)(
    ((-INF BLANK) _ r)
))
```

**RIGHT-EXTENSION**
```
((
    (l _ INF)
)(
    (l _ (BLANK INF))
))
```

where `BLANK` represents the symbol which we extend the tape with, typically a constant. In the extension rules, we append one blank symbol to the left or right ends of a tape when the head has reached the last possible read/write symbol.

In most cases we can safely take `-INF == INF` and set it to `()`, the empty group, which does not add to the ruleset constants. Handling blank symbols is more often a matter of which machine you want to simulate and which conventions you want to use.

## State and State-Transitions

To simulate states and state transitions, we consider a state expression (arbitrary) and a tape expression where we explicitly mark the current head with the transition table constant and then mark the future position of the head of the tape. Here's an example to illustrate:

```
((
    B ((l p) 1 r)
)(
    C (l p (0 r))
))
```

In this transition we go from state `B` to state `C` reading a `1` from the head, writing a `0` to the head and moving one step to the left, so that now `p` is at the head. The only way to match the hypothesis of this rule is for the tape to already have at least one read/write symbol on the left tape. If the left tape has only a terminal symbol, then it will never match this rule and we will need to first perform a **LEFT-EXTENSION** before we can match here.

For a right-moving rule, we match on a symbol on the right tape instead:

```
((
    D (l 0 (n r))
)(
    D ((l 1) n r)
))
```

like this transition from `D` to `D` reading a `0` writing a `1` and moving to the right.

## Initial and Halting States

To describe specific initial and halting state of a turing machine we can use the following two axioms:

```
(()(INIT INIT-TAPE))
(()(HALT HALT-TAPE))
```

If we use the left and right extension rules, fix a set of state transitions as above, and add the initial state axiom, then proving that a machine halts (with a given output tape) means proving that the halt axiom can be reached by composition using such a ruleset. This composition represents a computation trace of the turing machine, showing every state transition, symbol reading, symbol writing and tape extension that occured from the inital state to the halting state.

## Non-Determinism

To encode non-determinism we can add one rule for every possible transition out of a particular state-symbol reading, like the following:

```
((
    A (l 0 (n r))
)(
    B ((l 1) n r)
))
((
    A (l 0 (n r))
)(
    C ((l 0) n r)
))
((
    A ((l p) 0 r)
)(
    D (l p (1 r))
))
```

Notice that we fixed the state `A` and the symbol reading `0`, but in each matching rule we could either transition to a different state, write a different symbol, or move in a different direction.

## Multiple Tapes

Multi-tape turing machines, either with multiple computation tapes, or special input/output, or oracle tapes, can be encoded by adding one more element to the tape specification denoting the kind of tape:

```
(KIND l _ r)
```

Specific instances of multi-tape turing machines using these data-structures have not been explored yet.


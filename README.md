```
// work in progress //
```

# reasoner.js

_**tags:** metacompiling, program synthesis, automated logical reasoning_

```
project status:
    [ ] alpha conception
        [x] theorizing
        [ ] implementing
            [x] parsing input s-exprs
            [x] extensional reasoning
                [x] pattern matching
                [x] non-deterministic match involving multiple premises
                [x] non-deterministic match involving multiple conclusions
                [x] pairing input to output
            [ ] intensional reasoning
                [ ] untyped variables
                [ ] typed variables
                [ ] free variables
            [ ] nested rules
            [ ] sane error messages
    [ ] beta testing and revising code
    [ ] gamma release
```

Check out the current code performing at [online playground](https://symbolverse.github.io/reasoner.js/playground/).

## table of contents

- [1. about reasoner.js](#1-about-reasoner.js)
- [2. how does it work?](#2-how-does-it-work)
- [3. project specifics](#3-project-specifics)
- [4. work done so far](#4-work-done-so-far)
- [5. future plans](#5-future-plans)

## 1. about reasoner.js

*reasoner.js* is a [term graph rewriting](https://en.wikipedia.org/wiki/Graph_rewriting#Term_graph_rewriting) tool for transforming any input [s-expr](https://en.wikipedia.org/wiki/S-expression) to any output s-expr using its own [metalanguage](https://en.wikipedia.org/wiki/Metalanguage) as a [rule-based system](https://en.wikipedia.org/wiki/Rule-based_system). *reasoner.js* may be used for a wide range of computing tasks, but its main intentions are to support [metacompiling](https://en.wikipedia.org/wiki/Compiler-compiler), [program synthesis](https://en.wikipedia.org/wiki/Program_synthesis), and [automated logical reasoning](https://en.wikipedia.org/wiki/Automated_reasoning).

## 2. how does it work?

*reasoner.js* utilizes term graph rewriting process based on implicative and its dual, co-implicative rewriting. As implicative rewriting may yield a combinatorial explosion, we perform it in combination with co-implicative rewriting, thus reducing the search space in may cases. Combination of implicative and co-implicative rewriting also often avoids unnecessary infinite loops, automatically deliberating us from explicit recursion control in many situations.

In *reasoner.js*, we provide three sets of rules: one for input, one for output, and one for chaining between them. Input rule set branches from the source constant forwards to *concrete input*. Output rule set branches from the target constant backwards to *abstract output*. Chaining rules branch between input and output. If there exist a provable chaining connection between input and output then *concrete output* is back-propagated from the concrete input.

```
        source
          /\
        /\/\/\
      /  INPUT \
    /\/\/\/\/\/\/\
         CHAIN    
    \/\/\/\/\/\/\/
      \ OUTPUT /
        \/\/\/
          \/
        target
```

This is a symmetrical process, meaning that like we can ask what output yields certain input, we can also ask what input yields certain output. Answering these questions utilizes processes called forward and backward chaining, respectively.

## 3. project specifics

*reasoner.js* takes an input file, an arbitrary metaprogram, and constructs an output file from the input file using the metaprogram. The metaprogram is actually a set of formulas similar to those in mathematics with the difference that the *reasoner.js* formulas may transform not only math expressions, but also any kind of s-exprs.

To get a glimpse on how a *reasoner.js* metaprogram looks like, here's a quick example:

```
/*
    reasoner.js cat/dog decision example
*/

(
    // main rule
    RULE
    (
        READ
        (RGT (hearing <voice>))
        (FORE (LFT <voice>) (RGT barks))
        (FORE (LFT <voice>) (RGT meows))
    )
    (
        CHAIN
        (FORE (LFT (hearing meows)) (RGT (being cat)))
        (FORE (LFT (hearing barks)) (RGT (being dog)))
    )
    (
        WRITE
        (BACK (LFT cat) (RGT <living>))
        (BACK (LFT dog) (RGT <living>))
        (RGT (being <living>))
    )
)
```

This program does the following:

- feeding an input containing `(hearing meows)` writes an output containing `(being cat)`
- feeding an input containing `(hearing barks)` writes an output containing `(being dog)`
- feeding any other input yields an error message

## 4. work done so far

A lot of research is invested in conceptualisation of *reasoner.js*, and it is still heavily under construction. During its conceptualisation journey, it has been an agile experimenting project, advancing its theoretical background with each iteration. Curious readers may want to skim over [historical documents directory](https://github.com/symbolverse/reasoner.js/tree/master/history) that collect the successive iterations.

The current iteration is explained in actual [working draft](draft/reasoner.md), and its [implementation](https://symbolverse.github.io/reasoner.js/playground/) is in progress. Expect updates to working draft during implementation phase.

Related to *reasoner.js*, various experiments in Javascript were conducted with term rewriting concepts, finally achieving some promising results. Please refer to [rewrite.js repository](https://github.com/contrast-zone/rewrite.js) for more information about the latest experiment.

## 5. future plans

We are continuing our efforts to actively work on *reasoner.js*, hoping to get closer to finishing actual implementation.

```
// work in progress //
```


---
layout: post
title: "[ and [[ in shell script"
tags:
- shellscript
- bash
- linux
redirect_from: /2012/05/10/and-in-shell-script.html
---

終於搞懂在 shell script 裡 `[ ... ]` 和 `[[ ... ]]` 的差別了

    $ help [
    [: [ arg... ]
        This is a synonym for the "test" builtin, but the last
        argument must be a literal `]', to match the opening `['.
    [[ ... ]]: [[ expression ]]
        Returns a status of 0 or 1 depending on the evaluation of the conditional
        expression EXPRESSION.  Expressions are composed of the same primaries used
        by the `test' builtin, and may be combined using the following operators
        
            ( EXPRESSION )  Returns the value of EXPRESSION
            ! EXPRESSION    True if EXPRESSION is false; else false
            EXPR1 && EXPR2  True if both EXPR1 and EXPR2 are true; else false
            EXPR1 || EXPR2  True if either EXPR1 or EXPR2 is true; else false
        
        When the `==' and `!=' operators are used, the string to the right of the
        operator is used as a pattern and pattern matching is performed.  The
        && and || operators do not evaluate EXPR2 if EXPR1 is sufficient to
        determine the expression's value.

簡單的說就是要用 `&&`, `||`, `!`, `()` 的話要用 `[[ ... ]]`

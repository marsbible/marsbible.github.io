---
layout: post
title: lamda表达式 
modified: 2019-02-26
tags: [lambda]
categories: [技术]
---

### lamda表达式
#### Definition

Lambda expressions are composed of

* variables v1, v2, ..., vn, ...
* the abstraction symbols lambda 'λ' and dot '.'
* parentheses ( )

The set of lambda expressions, Λ, can be defined inductively:

1. If x is a variable, then x ∈ Λ
2. If x is a variable and M ∈ Λ, then (λx.M) ∈ Λ
3. If M, N ∈ Λ, then (M N) ∈ Λ

Instances of rule 2 are known as abstractions and instances of rule 3 are known as applications.

#### Notation

To keep the notation of lambda expressions uncluttered, the following conventions are usually applied.

* Outermost parentheses are dropped: M N instead of (M N)
* Applications are assumed to be left associative: M N P may be written instead of ((M N) P)
* The body of an abstraction extends as far right as possible: λx.M N means λx.(M N) and not (λx.M) N
* A sequence of abstractions is contracted: λx.λy.λz.N is abbreviated as λxyz.N

#### Free and bound variables

The abstraction operator, λ, is said to bind its variable wherever it occurs in the body of the abstraction. Variables that fall within the scope of an abstraction are said to be bound. All other variables are called free. For example, in the following expression y is a bound variable and x is free: λy.x x y. Also note that a variable is bound by its "nearest" abstraction. In the following example the single occurrence of x in the expression is bound by the second lambda: λx.y (λx.z x)

The set of free variables of a lambda expression, M, is denoted as FV(M) and is defined by recursion on the structure of the terms, as follows:

1. FV(x) = {x}, where x is a variable
2. FV(λx.M) = FV(M) \ {x}
3. FV(M N) = FV(M) ∪ FV(N)[14]

An expression that contains no free variables is said to be closed. Closed lambda expressions are also known as combinators and are equivalent to terms in combinatory logic.

#### Reduction

The meaning of lambda expressions is defined by how expressions can be reduced.[15]

There are three kinds of reduction:

* **α-conversion**: changing bound variables (**alpha**);
* **β-reduction**: applying functions to their arguments (**beta**);
* **η-conversion**: which captures a notion of extensionality (**eta**).

We also speak of the resulting equivalences: two expressions are β-equivalent, if they can be β-converted into the same expression, and α/η-equivalence are defined similarly.

The term redex, short for reducible expression, refers to subterms that can be reduced by one of the reduction rules. For example, (λx.M) N is a beta-redex in expressing the substitution of N for x in M; if x is not free in M, λx.M x is an eta-redex. The expression to which a redex reduces is called its reduct; using the previous example, the reducts of these expressions are respectively M[x:=N] and M.

#### α-conversion

Alpha-conversion, sometimes known as alpha-renaming,[16] allows bound variable names to be changed. For example, alpha-conversion of λx.x might yield λy.y. Terms that differ only by alpha-conversion are called α-equivalent. Frequently, in uses of lambda calculus, α-equivalent terms are considered to be equivalent.

The precise rules for alpha-conversion are not completely trivial. First, when alpha-converting an abstraction, the only variable occurrences that are renamed are those that are bound to the same abstraction. For example, an alpha-conversion of λx.λx.x could result in λy.λx.x, but it couldnot result in λy.λx.y. The latter has a different meaning from the original.

Second, alpha-conversion is not possible if it would result in a variable getting captured by a different abstraction. For example, if we replace xwith y in λx.λy.x, we get λy.λy.y, which is not at all the same.

In programming languages with static scope, alpha-conversion can be used to make name resolution simpler by ensuring that no variable name masks a name in a containing scope (see alpha renaming to make name resolution trivial).

In the De Bruijn index notation, any two alpha-equivalent terms are literally identical.

*Substitution*

Substitution, written E[V := R], is the process of replacing all free occurrences of the variable V in the expression E with expression R. Substitution on terms of the λ-calculus is defined by recursion on the structure of terms, as follows (note: x and y are only variables while M and N are any λ expression).

    x[x := N]        ≡ N
    y[x := N]        ≡ y, if x ≠ y
    (M1 M2)[x := N]  ≡ (M1[x := N]) (M2[x := N])
    (λx.M)[x := N]   ≡ λx.M
    (λy.M)[x := N]   ≡ λy.(M[x := N]), if x ≠ y, provided y ∉ FV(N)

To substitute into a lambda abstraction, it is sometimes necessary to α-convert the expression. For example, it is not correct for (λx.y)[y := x]to result in (λx.x), because the substituted x was supposed to be free but ended up being bound. The correct substitution in this case is (λz.x), up to α-equivalence. Notice that substitution is defined uniquely up to α-equivalence.

#### β-reduction

Beta-reduction captures the idea of function application. Beta-reduction is defined in terms of substitution: the beta-reduction of  ((λV.E) E′)  is E[V := E′].

For example, assuming some encoding of 2, 7, ×, we have the following β-reduction: ((λn.n×2) 7) → 7×2.

#### η-conversion

Eta-conversion expresses the idea of extensionality, which in this context is that two functions are the same if and only if they give the same result for all arguments. Eta-conversion converts between λx.(f x) and f whenever x does not appear free in f.

### Normal forms and confluence

For the untyped lambda calculus, β-reduction as a rewriting rule is neither strongly normalising nor weakly normalising.

However, it can be shown that β-reduction is confluent. (Of course, we are working up to α-conversion, i.e. we consider two normal forms to be equal, if it is possible to α-convert one into the other.)

Therefore, both strongly normalising terms and weakly normalising terms have a unique normal form. For strongly normalising terms, any reduction strategy is guaranteed to yield the normal form, whereas for weakly normalising terms, some reduction strategies may fail to find it.
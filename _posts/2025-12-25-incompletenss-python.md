---
layout: post

title: "The incompleteness theorems in python"

date: 2025-12-25
---

I never really studied logic, but I think I have interesting things to say about the basics.
Logic is the home of the incompleteness theorems,
saying that no matter what axioms you start with, as long as they don't contradict each other, you will always:

1. have statements that you can't prove and also can't disprove

2. be unable to _prove_ that your axioms don't contradict each other. In other words, the statement "there are no provable statements whose negation is also provable" will not be provable.

This way of stating things is a bit sloppy; a better person than me would explain in precise terms what it means to have axioms or to prove things or for things to be correct. I won't.
I will however say that there are many possible mathematical languages that have different axioms and can prove different things, and that for all of this to work, your language should be sophisticated enough to talk about python.

You should be able to express statements like "The python program
```python
a = 7
while a != 1:
    if a % 2 == 0:
        a = a // 2
    else:
        a = 3 * a + 1
```
halts", or "The python program
```python
while True:
    pass
```
halts". 
You probably know how to prove the first statement and disprove the second. You should also be able to express the statement "The python program
```python
a = 2
while True:
    b = a
    while b != 1:
        if b % 2 == 0:
            b = b // 2
        else:
            b = 3 * b + 1
        if b == a:
            break
    if b == a:
        break
    a += 1
```
halts", which you probably don't know how to prove or disprove.
Note that any code that halts has a proof that it halts; this proof is simply the step-by-step emulation of the code.

Let's translate everything about this into python. We can implement some python classes `Proof` and `Statement` whose objects encode proofs and statements. We can implement a function
```py
def statement_that_code_halts(code: Code) -> Statement
```
returning the _statement_ that a piece of python code halts.
Verifying proof correctness is a purely mechanical process, so we can implement a function
```python
def proof_is_correct(proof: Proof, statement: Statement) -> bool
```
performing it. Note that this function halts for every input.

Now, the first incompleteness theorem simply follows from the halting problem. Since proofs are finite sequences of text, encoded somehow, we can iterate through them.
Consider the function

```python
def does_code_halt(code: Code) -> bool:
    statement = statement_that_code_halts(code)
    for proof in Proof.__iter__():
        if proof_is_correct(proof, statement):
            return True
        if proof_is_correct(proof, not statement):
            return False
```

which seems to detect when a python code halts. You've heard of the halting problem, so you know that is impossible, but `does_code_halt` definitely seems to get the right answer for every piece of python code that possesses a proof or disproof for its halting. 
So, there must be some python code possessing no such proof or disproof.
This already demonstrates the first incompleteness theorem.

We can dive deeper. Consider the function
```python
def f():
    if does_code_halt("f()"):
        while True:
            pass
```
Let's prove that `f()` doesn't halt. Suppose for the sake of contradiction that it did halt. Then there is a proof that it halts, so the program `does_code_halt("f()")` halts and returns the value `True`. But this implies that the program `f()` would enter the infinite loop. Contradiction!

OK, we just proved that `f()` doesn't halt. 
Seems legit.
But the ability to find this proof is not reserved to human beings. The soul-less loop in `does_code_halt` will eventually find it, so `does_code_halt("f()")` is a program that halts and returns `False`. But this means `f()` must halt. Wait what?

This is the most convincing paradox I know!
I recommend appreciating it before spoiling the answer.

Here's what's actually happening.

A loop that iterates over all proofs will, indeed, find the above proof that `f()` doesn't halt.
If the loop inside `does_code_halt("f()")` gets to this proof, then `does_code_halt("f()")` will, indeed, halt and return `False`. 

But we failed to consider another possibility. 
What if there is a proof that `f()` doesn't halt, and _also_ a proof that `f()` halts?
In this scenario, the loop in `does_code_halt("f()")` could reach a proof that `f()` halts before reaching a proof `f()` doesn't halt,
making it return `True`. This genuinely ruins our proof that `f()` doesn't halt.

But surely there can't both be a proof that `f()` halts and a proof that it doesn't, right? That would mean the axioms we work with are contradictory. Surely there's no point in anything if the axioms are contradictory.
However, even if the axioms are fine, it doesn't mean they are _provably_ fine. Without a _proof_ that they don't contradict each other, we can't _prove_ that `does_code_halt("f()")` will return `False`.

So here is the proof of the second incompleteness theorem. Consider the statement "there are no statements with both a proof and a disproof"; we call this the consistency statement.
Suppose for the sake of contradiction that the consistency statement was provable.

In this case we genuinely have a proof `f()` doesn't halt, which looks like this:

Suppose for the sake of contradiction that `f()` halts. Then there is a proof that it halts. Because of the consistency statement, _which we can prove_, there exist no proofs that `f()` doesn't halt.
Thus, the loop inside `does_code_halt("f()")` would never find a proof `f()` doesn't halt, and eventually find a proof that it does.
So `does_code_halt("f()")` would return `True`,
in which case `f()` enters a loop; but we just assumed `f()` halts. 

The argument above is now a correct proof that `f()` doesn't halt. Using the consistency statement, _which we can prove_, there are no proofs that `f()` halts. Thus, the loop inside
`does_code_halt("f()")`
will never find a proof `f()` halts, and eventually find a proof that it doesn't. So `does_code_halt("f()")` will return `False`. But this means `f()` halts! This contradicts our initial assumption.

(Methodic pause)

The initial assumption was that the consistency statement is provable.
So we are done.

---

Should the incompleteness theorems change your worldview? Do they mean doing math is pointless? That the Riemann hypothesis is undecidable? That quantum effects make human brains superior to machines?
No, No, I'd bet against it, and No. But it's good math.
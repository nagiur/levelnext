# Higher-Order Functions `CH5`
## Abstraction [সংক্ষিপ্তসার]

In the context of programming, these kinds of vocabularies are usually called abstractions. Abstractions hide details and give us the ability to talk about problems at a higher (or more abstract) level. 
`[Pea soup recipes]`

> The second is shorter and easier to interpret. But you do need to understand a few more cooking-related words such as soak, simmer, chop, and, I guess, vegetable
## Abstracting repetition

```javascript
function repeat(n, action) {
  for (let i = 0; i < n; i++) {
    action(i);
  }
}
```
We don’t have to pass a predefined function to `repeat`. Often, it is easier to create a function value on the spot instead.
```javascript
let labels = [];
repeat(5, i => {
  labels.push(`Unit ${i + 1}`);
});
console.log(labels);
// → ["Unit 1", "Unit 2", "Unit 3", "Unit 4", "Unit 5"]
```
This is structured a little like a `for loop`—it first describes the kind of `loop` and then provides a `body`. However, the body is now written as a `function` value, which is wrapped in the parentheses of the call to `repeat`. This is why it has to be closed with the `closing brace and closing parenthesis`. In cases like this example, where the body is a single small expression, you could also omit the braces and write the loop on a single line.

## Higher-order functions
Functions that `operate on other functions`, either by `taking them as arguments` or `by returning them`, are called `higher-order functions`.

Higher-order functions allow us to `abstract over actions`, not just `values`. They come in `several forms`.

```javascript
function greaterThan(n) {
  return m => m > n;
}
let greaterThan10 = greaterThan(10);
console.log(greaterThan10(11));
// → true
```

And we can have functions that change other functions.
```javascript
function noisy(f) {
  return (...args) => {
    console.log("calling with", args);
    let result = f(...args);
    console.log("called with", args, ", returned", result);
    return result;
  };
}
noisy(Math.min)(3, 2, 1);
// → calling with [3, 2, 1]
// → called with [3, 2, 1] , returned 1
```

We can even write functions that provide new types of control flow.

```javascript
function unless(test, then) {
  if (!test) then();
}

repeat(3, n => {
  unless(n % 2 == 1, () => {
    console.log(n, "is even");
  });
});
// → 0 is even
// → 2 is even
```
There is a built-in array method, `forEach`, that provides something like a `for/of loop` as a `higher-order function`.
```javascript 
["A", "B"].forEach(l => console.log(l));
// → A
// → B
```

## Script data set
> One area where higher-order functions shine is data processing. To process data, we’ll need some actual data. This chapter will use a data set about scripts
> — writing systems such as Latin, Cyrillic, or Arabic.

```javascript
{
  name: "Coptic",
  ranges: [[994, 1008], [11392, 11508], [11513, 11520]],
  direction: "ltr",
  year: -200,
  living: false,
  link: "https://en.wikipedia.org/wiki/Coptic_alphabet"
}
```
The ranges property contains an array of Unicode character ranges, each of which is a two-element array containing a lower bound and an upper bound. Any character codes within these ranges are assigned to the script. The lower bound is inclusive (code 994 is a Coptic character), and the upper bound is non-inclusive (code 1008 isn’t).

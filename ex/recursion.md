see also [The Evolution of a Haskell Programmer](http://www.willamette.edu/~fruehr/haskell/evolution.html).

```haskell
fact :: Int -> Int
fact 0 = 1
fact n = n * fact (n-1)

main = print $ fact 7
```

```bash
$ runhaskell recursion.hs
5040
```

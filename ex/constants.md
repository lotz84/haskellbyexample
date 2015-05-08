All variables are immutable and constant.

```haskell
s :: String
s = "constant"

main = do
    putStrLn s

    let n = 500000000
    let d = 3e20 / n

    print d
    print $ sin n
```

```bash
$ runhaskell constants.hs
constant
6.0e11
-0.284704073237544
```

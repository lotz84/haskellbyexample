```haskell
plus :: Int -> Int -> Int
plus = (+)

plusPlus :: Int -> Int -> Int -> Int
plusPlus a b c = a + b + c

main = do
    let res = plus 1 2
    putStrLn $ "1+2 = " ++ show res

    let res = plusPlus 1 2 3
    putStrLn $ "1+2+3 = " ++ show res
```

```bash
$ runhaskell functions.hs
1+2 = 3
1+2+3 = 6
```

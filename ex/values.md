```haskell

main = do
    putStrLn $ "haskell " ++ "lang"
    putStrLn $ "1+1 = " ++ show (1+1)
    putStrLn $ "7.0/3.0 = " ++ show (7.0/3.0)

    print $ True && False
    print $ True || False
    print $ not True
```

```bash
$ runhaskell values.hs
haskell lang
1+1 = 2
7.0/3.0 = 2.3333333333333335
False
True
False
```

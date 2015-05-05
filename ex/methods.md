Just use functions.

```haskell
data Rect = Rect Int Int

area :: Rect -> Int
area (Rect w h) = w * h

perim :: Rect -> Int
perim (Rect w h) = 2 * w + 2 * h

main = do
    let r = Rect 10 5
    putStrLn $ "area: " ++ show (area r)
    putStrLn $ "perim: " ++ show (perim r)
```

```bash
$ runhaskell methods.hs
area: 50
perim: 30
```

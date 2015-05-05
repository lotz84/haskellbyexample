`forM_ = flip mapM_`

```haskell
import Control.Monad

main = do
    forM_ [1..3] $ \i -> do
        print i

    forM_ [7..9] $ \j -> do
        print j

    forM_ [1..] $ \_ -> do
        putStrLn "loop"
        error "break"
```

```bash
$ runhaskell for.hs
1
2
3
7
8
9
loop
for.hs: break
```

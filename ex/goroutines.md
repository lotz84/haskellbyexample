About forkIO.

```haskell
import Control.Monad
import Control.Concurrent

f :: String -> IO ()
f from = forM_ [0..2] (\i -> putStrLn $ from ++ ":" ++ show i)

main = do
    f "direct"
    forkIO $ f "forkIO"
    forkIO $ (\msg -> putStrLn msg) "going"

    getLine
    putStrLn "done"
```

```bash
$ runhaskell goroutines.hs
direct:0
direct:1
direct:2
forkIO:0
forkIOg:o1i
ngf
orkIO:2
<enter>
done
```

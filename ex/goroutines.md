Actually there is no goroutine in Haskell.  
Instead, This example uses forkIO.  
It is a common way to use concurrency in Haskell.

```haskell
import Control.Monad
import Control.Concurrent

f :: String -> IO ()
f from = forM_ [0..2] (\i -> putStrLn $ from ++ ":" ++ show i)

main = do
    f "direct"
    forkIO $ f "forkIO"
    forkIO $ (\msg -> putStrLn msg) "going"

    threadDelay 1000000
    putStrLn "done"
```

```bash
$ runhaskell goroutines.hs
direct:0
direct:1
direct:2
going
forkIO:0
forkIO:1
forkIO:2
done
```

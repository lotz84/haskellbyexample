```haskell
import Control.Concurrent

worker :: MVar Bool -> IO ()
worker done = do
    putStr "working..."
    threadDelay 1000000
    putStrLn "done"

    putMVar done True

main = do
    done <- newEmptyMVar
    forkIO $ worker done

    takeMVar done
    return ()
```

```bash
$ runhaskell channel-synchronization.hs
working...done
```

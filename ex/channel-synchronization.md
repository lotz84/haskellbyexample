```haskell
import Control.Concurrent
import Control.Concurrent.STM

worker :: TMVar Bool -> IO ()
worker done = do
    putStr "working..."
    threadDelay 1000000

    putStrLn "done"
    atomically $ putTMVar done True

main = do
    done <- atomically newEmptyTMVar
    forkIO $ worker done

    atomically $ takeTMVar done
    return ()
```

```bash
$ runhaskell channel-synchronization.hs
working...done
```

```haskell
import Control.Monad
import Control.Concurrent
import Control.Concurrent.STM
 
main = do
    ops <- atomically $ newTVar 0
    forM_ [0..49] $ \_ -> do
        forkIO . forever $ do
            atomically $ do
                x <- readTVar ops
                writeTVar ops (x + 1)
            threadDelay 100
    threadDelay 1000000
    opsFinal <- atomically $ readTVar ops
    putStrLn $ "ops: " ++ show opsFinal
```

```bash
$ runhaskell atomic-counters.hs
ops: 146555
```

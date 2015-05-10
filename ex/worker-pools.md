```haskell
import Control.Monad
import Control.Concurrent
import Control.Concurrent.STM

worker :: Int -> TQueue Int -> TQueue Int -> MVar () -> IO ()
worker n jobs results lock = forever $ do
    j <- atomically $ readTQueue jobs
    withMVar lock $ \_ -> do
        putStrLn $ "worker " ++ show n ++ " processing job " ++ show j
    threadDelay (1 * 1000000)
    atomically $ writeTQueue results (2 * j)

main = do
    jobs <- atomically $ newTQueue
    results <- atomically $ newTQueue
    lock <- newMVar ()

    forM_ [1..3] $ \w -> do
        forkIO $ worker w jobs results lock

    forM_ [1..9] $ atomically . writeTQueue jobs
    forM_ [1..9] $ \_ -> atomically $ readTQueue results
```

```bash
$ runhaskell worker-pools.hs
worker 1 processing job 1
worker 2 processing job 2
worker 3 processing job 3
worker 2 processing job 4
worker 3 processing job 5
worker 1 processing job 6
worker 3 processing job 7
worker 1 processing job 8
worker 2 processing job 9
```

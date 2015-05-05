```haskell
import Control.Concurrent
import Control.Monad

worker :: Int -> Chan Int -> Chan Int -> MVar () -> IO ()
worker n jobs results mutex = forever $ do
    j <- readChan jobs
    withMVar mutex $ \_ -> do
        putStrLn $ "worker " ++ show n ++ " processing job " ++ show j
    threadDelay (1 * 1000000)
    writeChan results (2 * j)

main = do
    jobs <- newChan
    results <- newChan
    mutex <- newMVar ()

    forM_ [1..3] $ \w -> do
        forkIO $ worker w jobs results mutex

    forM_ [1..9] $ writeChan jobs
    forM_ [1..9] $ \_ -> readChan results
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

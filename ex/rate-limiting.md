```haskell
import Data.Time
import Control.Monad
import Control.Concurrent
import Control.Concurrent.STM

main = do
    requests <- atomically $ do
        req <- newTQueue
        mapM_ (writeTQueue req) [1..5]
        return req

    limitter <- atomically $ newEmptyTMVar
    forkIO . forever $ do
        atomically $ putTMVar limitter ()
        threadDelay (200 * 1000)

    let loop1 = do
        req <- atomically $ do
            r <- readTQueue requests
            takeTMVar limitter
            return r
        now <- getCurrentTime
        putStrLn $ "request " ++ show req ++ " " ++ show now
        isEmpty <- atomically $ isEmptyTQueue requests
        if isEmpty
            then return ()
            else loop1
    loop1

    now <- getCurrentTime
    burstyLimitter <- atomically $ do
        limitter <- newTBQueue 3
        forM_ [0..2] $ \_ -> writeTBQueue limitter now
        return limitter

    forkIO . forever $ do
        now <- getCurrentTime
        atomically $ writeTBQueue burstyLimitter now
        threadDelay (200 * 1000)

    burstyRequests <- atomically $ do
        req <- newTQueue
        mapM_ (writeTQueue req) [1..5]
        return req

    let loop2 = do
        req <- atomically $ do
            r <- readTQueue burstyRequests
            readTBQueue burstyLimitter
            return r
        now <- getCurrentTime
        putStrLn $ "request " ++ show req ++ " " ++ show now
        isEmpty <- atomically $ isEmptyTQueue burstyRequests
        if isEmpty
            then return ()
            else loop2
    loop2
```

```bash
$ runhaskell rate-limiting.hs
request 1 2018-10-18 21:22:30.873766 UTC
request 2 2018-10-18 21:22:31.074185 UTC
request 3 2018-10-18 21:22:31.274635 UTC
request 4 2018-10-18 21:22:31.474943 UTC
request 5 2018-10-18 21:22:31.675533 UTC
request 1 2018-10-18 21:22:31.675713 UTC
request 2 2018-10-18 21:22:31.675839 UTC
request 3 2018-10-18 21:22:31.675947 UTC
request 4 2018-10-18 21:22:31.676057 UTC
request 5 2018-10-18 21:22:31.876147 UTC
```

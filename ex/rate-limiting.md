```haskell
import Data.Time
import Control.Concurrent
import Control.Monad

main = do
    requests <- newChan
    mapM_ (writeChan requests) [1..5]

    limitter <- newChan
    forkIO . forever $ do
        writeChan limitter ()
        threadDelay (200 * 1000)

    forM_ [1..5] $ \req -> do
        readChan limitter
        now <- getCurrentTime
        putStrLn $ "request " ++ show req ++ " " ++ show now

    burstyLimitter <- newChan
    now <- getCurrentTime
    forM_ [0..2] $ \_ -> do
        writeChan burstyLimitter now

    forkIO . forever $ do
        now <- getCurrentTime
        writeChan burstyLimitter now
        threadDelay (200 * 1000)

    forM_ [1..5] $ \req -> do
        readChan burstyLimitter
        now <- getCurrentTime
        putStrLn $ "request " ++ show req ++ " " ++ show now
```

```bash
$ runhaskell rate-limiting.hs
request 1 2015-05-05 12:44:43.840436 UTC
request 2 2015-05-05 12:44:44.04126 UTC
request 3 2015-05-05 12:44:44.246552 UTC
request 4 2015-05-05 12:44:44.451247 UTC
request 5 2015-05-05 12:44:44.654298 UTC
request 1 2015-05-05 12:44:44.654908 UTC
request 2 2015-05-05 12:44:44.655353 UTC
request 3 2015-05-05 12:44:44.655698 UTC
request 4 2015-05-05 12:44:44.656051 UTC
request 5 2015-05-05 12:44:44.86019 UTC
```

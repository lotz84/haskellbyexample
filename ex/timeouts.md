```haskell
{-# LANGUAGE GADTs #-}
import Control.Concurrent

main = do
    c1 <- newEmptyMVar
    forkIO $ do
        threadDelay (2 * 1000000)
        putMVar c1 "result 1"

    t1 <- newTimer (1 * 1000000)
    select [ Case c1 $ \res -> putStrLn res
           , Case t1 $ \_   -> putStrLn "timeout 1"]

    c2 <- newEmptyMVar
    forkIO $ do
        threadDelay (2 * 1000000)
        putMVar c2 "result 2"

    t2 <- newTimer (3 * 1000000)
    select [ Case c2 $ \res -> putStrLn res
           , Case t2 $ \_   -> putStrLn "timeout 2"]

type Timer = MVar ()

newTimer :: Int -> IO Timer
newTimer delay = do
    timer <- newEmptyMVar
    forkIO $ do
        threadDelay delay
        putMVar timer ()
    return timer

data Select a where
    Default :: IO a -> Select a
    Case    :: MVar b -> (b -> IO a) -> Select a

select :: [Select a] -> IO a
select [] = error "select: empty list"
select ((Default x):_) = x
select (x@(Case v f):xs)  = do
    var <- tryTakeMVar v
    case var of
        Just b  -> f b
        Nothing -> select (xs ++ [x])
```

```bash
$ runhaskell timeouts.hs
timeout 1
result 2
```

```haskell
{-# LANGUAGE GADTs #-}
import Control.Concurrent
import Control.Concurrent.STM

main = do
    c1 <- atomically $ newTQueue
    forkIO $ do
        threadDelay (2 * 1000000)
        atomically $ writeTQueue c1 "result 1"

    t1 <- newTimer (1 * 1000000)
    select [ Case c1 $ \res -> putStrLn res
           , Case t1 $ \_   -> putStrLn "timeout 1"]

    c2 <- atomically $ newTQueue
    forkIO $ do
        threadDelay (2 * 1000000)
        atomically $ writeTQueue c2 "result 2"

    t2 <- newTimer (3 * 1000000)
    select [ Case c2 $ \res -> putStrLn res
           , Case t2 $ \_   -> putStrLn "timeout 2"]

type Timer = TMVar ()

newTimer :: Int -> IO Timer
newTimer delay = do
    timer <- atomically newEmptyTMVar
    forkIO $ do
        threadDelay delay
        atomically $ putTMVar timer ()
    return timer

class Selectable f where
    tryRead :: f a -> STM (Maybe a)

instance Selectable TMVar where
    tryRead = tryReadTMVar

instance Selectable TQueue where
    tryRead = tryReadTQueue

data Select a where
    Default :: IO a -> Select a
    Case    :: Selectable s => s b -> (b -> IO a) -> Select a

select :: [Select a] -> IO a
select [] = error "select: empty list"
select ((Default x):_) = x
select (x@(Case v f):xs)  = do
    var <- atomically $ tryRead v
    case var of
        Just b  -> f b
        Nothing -> select (xs ++ [x])
```

```bash
$ runhaskell timeouts.hs
timeout 1
result 2
```

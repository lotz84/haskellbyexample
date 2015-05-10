```haskell
{-# LANGUAGE GADTs #-}
import Control.Concurrent
import Control.Concurrent.STM

main = do
    messages <- atomically $ newEmptyTMVar :: IO (TMVar String)
    signals  <- atomically $ newEmptyTMVar :: IO (TMVar Bool)

    trymsg <- atomically $ tryReadTMVar messages
    case trymsg of
        Just m -> putStrLn $ "received message " ++ m
        Nothing -> putStrLn "no message received"

    let msg = "hi"
    success <- atomically $ tryPutTMVar messages msg
    if success
        then putStrLn $ "sent message " ++ msg
        else putStrLn "no message sent"

    select [ Case messages $ \msg -> putStrLn $ "received message " ++ msg
           , Case signals  $ \sig -> putStrLn $ "received signal " ++ show sig
           , Default $ putStrLn "no activiry"
           ]

class Selectable f where
    tryRead :: f a -> STM (Maybe a)

instance Selectable TMVar where
    tryRead = tryReadTMVar

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
$ runhaskell non-blocking-channel-operations.hs
no message received
sent message hi
received message hi
```

```haskell
{-# LANGUAGE GADTs #-}
import Control.Concurrent

main = do
    messages <- newEmptyMVar :: IO (MVar String)
    signals  <- newEmptyMVar :: IO (MVar Bool)

    trymsg <- tryTakeMVar messages
    case trymsg of
        Just m -> putStrLn $ "received message " ++ m
        Nothing -> putStrLn "no message received"

    let msg = "hi"
    success <- tryPutMVar messages msg
    if success
        then putStrLn $ "sent message " ++ msg
        else putStrLn "no message sent"

    select [ Case messages $ \msg -> putStrLn $ "received message " ++ msg
           , Case signals  $ \sig -> putStrLn $ "received signal " ++ show sig
           , Default $ putStrLn "no activiry"
           ] 

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
$ runhaskell non-blocking-channel-operations.hs
no message received
sent message hi
received message hi
```

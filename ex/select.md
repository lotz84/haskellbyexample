```haskell
{-# LANGUAGE GADTs #-}
import Control.Monad
import Control.Concurrent

main = do
    c1 <- newEmptyMVar
    c2 <- newEmptyMVar

    forkIO $ do
        threadDelay (1 * 1000000)
        putMVar c1 "one"

    forkIO $ do
        threadDelay (2 * 1000000)
        putMVar c2 "two"

    forM_ [0..1] $ \i ->
        select [ Case c1 $ \msg1 -> putStrLn $ "received " ++ msg1
               , Case c2 $ \msg2 -> putStrLn $ "received " ++ msg2
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
$ time runhaskell select.hs
received one
received two

real    0m2.266s
```

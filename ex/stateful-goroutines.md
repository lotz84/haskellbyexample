```haskell
{-# LANGUAGE GADTs #-}
import Control.Monad
import Control.Concurrent
import Control.Concurrent.STM
import System.Random
import Data.IORef
import qualified Data.Map as Map

data ReadOp = ReadOp { readKey  :: Int
                     , readResp :: MVar Int
                     }

data WriteOp = WriteOp { writeKey  :: Int
                       , writeVal  :: Int
                       , writeResp :: MVar Bool
                       }

main = do
    ops <- atomically $ newTVar 0 

    reads  <- newEmptyMVar
    writes <- newEmptyMVar

    forkIO $ do
        state <- newIORef Map.empty
        forever $ do
            select [ Case reads  $ \read  -> do
                        s <- readIORef state
                        let val = maybe 0 id $ Map.lookup (readKey read) s
                        putMVar (readResp read) val                
                   , Case writes $ \write -> do
                        modifyIORef state (Map.insert (writeKey write) (writeVal write))
                        putMVar (writeResp write) True
                   ]

    forM_ [0..99] $ \_ -> do
        forkIO . forever $ do
            key  <- randomRIO (0,4)
            resp <- newEmptyMVar
            let read = ReadOp { readKey = key, readResp = resp }
            putMVar reads read
            takeMVar resp

            atomically $ do
                x <- readTVar ops
                writeTVar ops (x + 1)

    forM_ [0..9] $ \_ -> do
        forkIO . forever $ do
            key  <- randomRIO (0,4)
            val  <- randomRIO (0,99)
            resp <- newEmptyMVar
            let write = WriteOp { writeKey = key, writeVal = val, writeResp = resp }
            putMVar writes write
            takeMVar resp

            atomically $ do
                x <- readTVar ops
                writeTVar ops (x + 1)

    threadDelay 1000000

    opsFinal <- atomically $ readTVar ops
    putStrLn $ "ops: " ++ show opsFinal

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
$ runhaskell stateful-goroutines.hs
ops: 11605
```

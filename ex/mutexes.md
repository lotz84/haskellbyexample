```haskell
import Control.Monad
import Control.Concurrent
import Control.Concurrent.STM
import System.Random
import Data.IORef
import Data.Maybe
import qualified Data.Map as Map
 
main = do
    state <- newMVar Map.empty
    ops <- atomically $ newTVar 0

    forM_ [0..99] $ \_ -> do
        total <- newIORef 0
        forkIO . forever $ do
            key <- randomRIO (0, 4) :: IO Int
            s <- takeMVar state
            let v = maybe 0 id $ Map.lookup key s
            modifyIORef total(+v)
            putMVar state s
            atomically $ do
                x <- readTVar ops
                writeTVar ops (x + 1)

    forM_ [0..9] $ \_ -> do
        forkIO . forever $ do
            key <- randomRIO (0, 4) :: IO Int
            val <- randomRIO (0, 99) :: IO Int
            s <- takeMVar state
            let s' = Map.insert key val s
            putMVar state s'
            atomically $ do
                x <- readTVar ops
                writeTVar ops (x + 1)
            

    threadDelay 1000000
    opsFinal <- atomically $ readTVar ops
    putStrLn $ "ops: " ++ show opsFinal

    s <- takeMVar state
    putStrLn $ "state: " ++ show s
```

```bash
$ runhaskell mutexes.hs
ops: 295124
state: fromList [(0,26),(1,66),(2,75),(3,96),(4,63)]
```

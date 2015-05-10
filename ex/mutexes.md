```haskell
import Control.Monad
import Control.Concurrent
import Control.Concurrent.STM
import Data.Maybe
import qualified Data.Map as Map
import System.Random

main = do
    state <- atomically $ newTVar Map.empty
    ops   <- atomically $ newTVar 0

    forM_ [0..99] $ \_ -> do
        total <- atomically $ newTVar 0
        forkIO . forever $ do
            key <- randomRIO (0, 4) :: IO Int
            atomically $ do
                s <- readTVar state
                writeTVar state s
                let v = maybe 0 id $ Map.lookup key s
                modifyTVar total (+v)
                modifyTVar ops (+1)

    forM_ [0..9] $ \_ -> do
        forkIO . forever $ do
            key <- randomRIO (0, 4) :: IO Int
            val <- randomRIO (0, 99) :: IO Int
            atomically $ do
                modifyTVar state (Map.insert key val)
                modifyTVar ops (+1)

    threadDelay 1000000
    opsFinal <- atomically $ readTVar ops
    putStrLn $ "ops: " ++ show opsFinal

    s <- atomically $ readTVar state
    putStrLn $ "state: " ++ show s
```

```bash
$ runhaskell mutexes.hs
ops: 299042
state: fromList [(0,50),(1,81),(2,55),(3,87),(4,47)]
```

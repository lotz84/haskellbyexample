```haskell
import Control.Concurrent
import Control.Concurrent.STM

main = do
    messages <- newQueue

    forkIO $ writeQueue messages "ping"

    msg <- readQueue messages
    putStrLn msg

    where
    newQueue   = atomically newTQueue
    readQueue  = atomically . readTQueue
    writeQueue = (atomically.) . writeTQueue
```

```bash
$ runhaskell channels.hs
ping
```

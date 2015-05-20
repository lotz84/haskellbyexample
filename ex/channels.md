```haskell
import Control.Concurrent
import Control.Concurrent.STM

main = do
    messages <- atomically newTQueue

    forkIO $ atomically $ writeTQueue messages "ping"

    msg <- atomically $ readTQueue messages
    putStrLn msg
```

```bash
$ runhaskell channels.hs
ping
```

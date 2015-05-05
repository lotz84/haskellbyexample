```haskell
import Control.Concurrent

main = do
    messages <- newEmptyMVar

    forkIO $ putMVar messages "ping"

    msg <- takeMVar messages
    putStrLn msg
```

```bash
$ runhaskell channels.hs
ping
```

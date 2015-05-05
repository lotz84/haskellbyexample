```haskell
import Control.Concurrent

main = do
    messages <- newChan

    writeChan messages "buffered"
    writeChan messages "channel"

    putStrLn =<< readChan messages
    putStrLn =<< readChan messages
```

```bash
$ runhaskell channel-buffering.hs
buffered
channel
```

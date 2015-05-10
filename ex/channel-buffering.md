You can treat [TQueue](http://hackage.haskell.org/package/stm/docs/Control-Concurrent-STM-TQueue.html) as an unbounded FIFO channel.

```haskell
import Control.Concurrent.STM

main = do
    messages <- atomically $ do
        msg <- newTQueue
        writeTQueue msg "buffered"
        writeTQueue msg "queue"
        return msg

    putStrLn =<< (atomically . readTQueue) messages
    putStrLn =<< (atomically . readTQueue) messages
```

```bash
$ runhaskell channel-buffering.hs
buffered
queue
```

see also [privileged-concurrency](http://hackage.haskell.org/package/privileged-concurrency)

```haskell
import Control.Concurrent

ping :: WriteOnly w => w String -> String -> IO ()
ping pings msg = write' pings msg

pong :: (ReadOnly r, WriteOnly w) => r String -> w String -> IO ()
pong pings pongs = do
    msg <- read' pings
    write' pongs msg

main = do
    pings <- newEmptyMVar
    pongs <- newEmptyMVar
    ping pings "passed message"
    pong pings pongs
    putStrLn =<< takeMVar pongs

class ReadOnly f where
    read' :: f a -> IO a
instance ReadOnly MVar where
    read' = takeMVar

class WriteOnly f where
    write' :: f a -> a ->  IO ()
instance WriteOnly MVar where
    write' = putMVar
```

```bash
$ runhaskell channel-directions.hs
passed message
```

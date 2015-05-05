To work this example, you need install [io-stream](https://hackage.haskell.org/package/io-streams).

```bash
$ cabal install io-stream
```

```haskell
import Control.Concurrent
import Control.Monad.IO.Class
import Data.Time
import Data.IORef
import System.IO.Streams (InputStream, makeOutputStream, fromGenerator, connect)
import qualified System.IO.Streams as Streams

main = do
    ticker <- newTicker (500 * 1000)
    output <- makeOutputStream $ \m -> case m of
                  Just now -> print now
                  Nothing  -> return ()
    forkIO $ (snd ticker) `connect` output

    threadDelay (1500 * 1000)
    stopTicker ticker
    putStrLn "Ticker stopped"

data State  = Start | Stop
type Ticker = (IORef State, InputStream UTCTime)

newTicker :: Int -> IO Ticker
newTicker delay = do
    state  <- newIORef Start
    stream <- fromGenerator (gen state)
    return (state, stream)
    where
    gen state = do
        runState <- liftIO $ readIORef state
        case runState of
            Start -> do
                now <- liftIO getCurrentTime
                Streams.yield now
                liftIO $ threadDelay delay
                gen state
            Stop -> return ()

stopTicker :: Ticker -> IO ()
stopTicker (state, _) = writeIORef state Stop
```

```bash
$ runhaskell tickers.hs
2015-05-05 12:30:57.82369 UTC
2015-05-05 12:30:58.330511 UTC
2015-05-05 12:30:58.834687 UTC
Ticker stopped
```

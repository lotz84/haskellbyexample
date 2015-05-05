```haskell
import Data.IORef
import Control.Concurrent

main = do
    timer1 <- newTimer (2 * 1000000)
    waitTimer timer1
    putStrLn "Timer 1 expired"

    timer2 <- newTimer (1 * 1000000)
    forkIO $ do
        waitTimer timer2
        putStrLn "Timer 2 expired"
    stopTimer timer2
    putStrLn "Timer 2 stopped"

data State = Start | Stop
type Timer = (IORef State, MVar ())

newTimer :: Int -> IO Timer
newTimer n = do
    state <- newIORef Start
    timer <- newEmptyMVar
    forkIO $ do
        threadDelay n
        runState <- readIORef state
        case runState of
            Start -> putMVar timer ()
            Stop  -> return ()
    return (state, timer)

waitTimer :: Timer -> IO ()
waitTimer (state, timer) = do
    runState <- readIORef state
    case runState of
        Start -> readMVar timer
        Stop  -> return ()

stopTimer :: Timer -> IO ()
stopTimer (state, _) = writeIORef state Stop
```

```bash
$ runhaskell timers.hs
Timer 1 expired
Timer 2 stopped
```

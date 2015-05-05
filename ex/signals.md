```haskell
import System.Exit
import System.Posix.Signals
import Control.Concurrent

main :: IO ()
main = do
    done <- newEmptyMVar
    let handler = do
            putStrLn ""
            putStrLn "interrupt"
            putMVar done ()
    installHandler keyboardSignal (Catch handler) Nothing
    putStrLn "awaiting signal"
    takeMVar done
    putStrLn "exiting"
```

```bash
$ runhaskell signals.hs
awaiting signal
^C
interrupt
exiting
```

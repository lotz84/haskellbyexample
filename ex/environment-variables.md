```haskell
import System.Environment

main = do
    setEnv "FOO" "1"
    putStr "FOO:" >> (putStrLn  =<< getEnv "FOO")
    putStr "BAR:" >> (print  =<< lookupEnv "BAR")
    putStrLn ""
    mapM_ putStrLn =<< map fst `fmap` getEnvironment
```

```bash
$ runhaskell environment-variables.hs
FOO:1
BAR:Nothing

MANPATH
rvm_bin_path
TERM_PROGRAM
...

$ BAR=2 runhaskell environment-variables.hs
FOO:1
BAR:Just "2"
...
```

```haskell
import System.Environment

main = do
    args <- getArgs
    let arg = args !! 3

    print args
    putStrLn arg

    progName <- getProgName
    putStrLn progName
```

```bash
$ ghc command-line-arguments.hs -o command-line-arguments
$ ./command-line-arguments a b c d
["a","b","c","d"]
d
command-line-arguments
```

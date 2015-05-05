```haskell
import System.Exit

main = do 
    exitWith $ ExitFailure 3
    putStrLn "!"
```

```bash
$ runhaskell exit.hs

$ ghc exit.hs -o exit
$ ./exit
$ echo $?
3
```

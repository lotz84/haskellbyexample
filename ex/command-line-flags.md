```haskell
import Data.Maybe
import System.Environment
import System.Console.GetOpt

data Opt = Help
         | Word String
         | Numb Int
         | Fork Bool
         | Svar String
         deriving (Eq, Show)

main = do
    args <- getArgs
    let options = [ Option ['h'] ["help"] (NoArg Help) "show help"
                  , Option [] ["word"] (OptArg (maybe (Word "foo") Word) "foo") "a string"
                  , Option [] ["numb"] (OptArg (maybe (Numb 42) (Numb . read)) "42") "an int"
                  , Option [] ["fork"] (OptArg (maybe (Fork False) (Fork . read)) "false") "a bool"
                  , Option [] ["svar"] (OptArg (maybe (Svar "bar") Svar) "bar") "a string var"
                  ]
    let (opts, rem, _) = getOpt Permute options args
    let putOpt opt = case opt of
                       Help     -> putStrLn $ usageInfo "" options
                       (Word x) -> putStrLn $ "word: " ++ x
                       (Numb x) -> putStrLn $ "numb: " ++ show x
                       (Fork x) -> putStrLn $ "fork: " ++ show x
                       (Svar x) -> putStrLn $ "svar: " ++ show x
    if null (filter (==Help) opts)
        then do
            mapM_ print opts
            putStrLn $ "tail: " ++ show rem

        else putStrLn $ usageInfo "" options
```

```bash
$ ghc command-line-flags.hs -o command-line-flags

$ ./command-line-flags --word=opt --numb=7 --fork --svar=flag
Word "opt"
Numb 7
Fork False
Svar "flag"
tail: []

$ ./command-line-flags --word=opt
Word "opt"
tail: []

$ ./command-line-flags --word=opt a1 a2 a3
Word "opt"
tail: ["a1","a2","a3"]

$ ./command-line-flags --word=opt a1 a2 a3 --numb=7
Word "opt"
Numb 7
tail: ["a1","a2","a3"]

$ ./command-line-flags -h

  -h  --help          show help
      --word[=foo]    a string
      --numb[=42]     an int
      --fork[=false]  a bool
      --svar[=bar]    a string var

$ ./command-line-flags --wat
tail: []
```

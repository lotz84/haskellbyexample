To work this example, you need install [optparse-declarative](http://hackage.haskell.org/package/optparse-declarative).

```bash
$ cabal install optparse-declarative
```

```haskell
{-# LANGUAGE DataKinds #-}
import Options.Declarative
import Control.Monad.IO.Class (liftIO)

flags :: Flag "" '["word"] "\"foo\"" "a string"     (Def "foo" String)
      -> Flag "" '["numb"] "42"      "an int"       (Def "42" Int)
      -> Flag "" '["fork"] "false"   "a bool"       Bool
      -> Flag "" '["svar"] "\"bar\"" "a string var" (Def "bar" String)
      -> Arg "ARGS" [String]
      -> Cmd "Command-Line Flags" ()
flags word numb fork svar args = do
    liftIO . putStrLn $ "word:" ++ get word
    liftIO . putStrLn $ "numb:" ++ (show . get $ numb)
    liftIO . putStrLn $ "fork:" ++ (show . get $ fork)
    liftIO . putStrLn $ "svar:" ++ get svar
    liftIO . putStrLn $ "tail:" ++ (show . get $ args)

main :: IO ()
main = run_ flags
```

```bash
$ ghc command-line-flags.hs -o command-line-flags

$ ./command-line-flags --word=opt --numb=7 --fork --svar=flag
word:opt
numb:7
fork:True
svar:flag
tail:[]

$ ./command-line-flags --word=opt
word:opt
numb:42
fork:False
svar:bar
tail:[]

$ ./command-line-flags --word=opt a1 a2 a3
word:opt
numb:42
fork:False
svar:bar
tail:["a1","a2","a3"]

$ ./command-line-flags --word=opt a1 a2 a3 --numb=7
word:opt
numb:42
fork:False
svar:bar
tail:["a1","a2","a3","--numb=7"]

$ ./command-line-flags -?
Usage: command-line-flags [OPTION...] ARGS
  Command-Line Flags

Options:
         --word="foo"   a string
         --numb=42      an int
         --fork         a bool
         --svar="bar"   a string var
  -?     --help         display this help and exit
  -v[n]  --verbose[=n]  set verbosity level

$ ./command-line-flags --wat
command-line-flags: unrecognized option '--wat'
Try 'command-line-flags --help' for more information.
```

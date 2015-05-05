```haskell
import System.IO
import System.Process

main = do
    (_, Just hout, _, _) <- createProcess (proc "date" []){ std_out = CreatePipe }
    dateOut <- hGetContents hout
    putStrLn "> date"
    putStrLn dateOut

    (Just hin, Just hout, _, _) <- createProcess (proc "grep" ["hello"]){ std_in = CreatePipe, std_out = CreatePipe }
    hPutStr hin "hello grep\ngoodbye grep"
    grepBytes <- hGetContents hout
    putStrLn "> grep hello"
    putStrLn grepBytes
    
    (_, Just hout, _, _) <- createProcess (proc "bash" ["-c", "ls -a -l -h"]){ std_out = CreatePipe }
    lsOut <- hGetContents hout
    putStrLn "> ls -a -l -h"
    putStrLn lsOut
```

```bash
$ runhaskell spawning-processes.hs
> date
2015年 5月 5日 火曜日 22時58分59秒 JST

> grep hello
hello grep

> ls -a -l -h
total 8
drwxr-xr-x   3  staff   102B  5  5 22:58 .
drwxr-xr-x  63  staff   2.1K  5  5 22:58 ..
-rw-r--r--   1  staff   639B  5  5 22:58 spawning-processes.hs
```

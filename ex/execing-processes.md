```haskell
import System.Process
import System.Directory

main = do
    path <- findExecutable "ls"
    case path of
        Nothing -> error "ls doesn't exist"
        Just _  -> createProcess (proc "ls" ["-a", "-l", "-h"])
```

```bash
$ runhaskell execing-processes.hs
total 8
drwxr-xr-x   3  staff   102B  5  5 23:00 .
drwxr-xr-x  63  staff   2.1K  5  5 22:58 ..
-rw-r--r--   1  staff   214B  5  5 23:00 execing-processes.hs
```

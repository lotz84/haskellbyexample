Use bracket.

```haskell
import Control.Exception
import System.IO
import System.IO.Error

main = bracket (createFile "/tmp/defer.txt") closeFile writeFile'

createFile :: FilePath -> IO Handle
createFile path = do
    putStrLn "creating"
    openFile path WriteMode `catch` (error . ioeGetErrorString)

writeFile' :: Handle -> IO ()
writeFile' handle = do
    putStrLn "writing"
    hPutStr handle "data"

closeFile :: Handle -> IO ()
closeFile handle = do
    putStrLn "closing"
    hClose handle
```

```bash
$ runhaskell defer.hs
creating
writing
closing
```

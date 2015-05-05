```haskell
import Foreign.Marshal.Array
import Control.Exception
import Data.Int
import System.IO

main = do
    let d1 = "hello\nhaskell\n"
    writeFile "/tmp/dat1" d1

    bracket (openBinaryFile "/tmp/dat2" WriteMode)
            (\handle -> hClose handle)
            $ \handle -> do
                let d2 = [115, 111, 109, 101, 10] :: [Int8]
                p <- newArray d2
                hPutBuf handle p (length d2)
                putStrLn $ "wrote " ++ show (length d2) ++ " bytes"

                hPutStr handle "writes\n"
                putStrLn "wrote 7 bytes"

                hSetBuffering handle (BlockBuffering Nothing) -- default
                hPutStr handle "buffered\n"
                putStrLn "wrote 9 bytes"
                hFlush handle
```

```bash
$ runhaskell writing-files.hs
wrote 5 bytes
wrote 7 bytes
wrote 9 bytes

$ cat /tmp/dat1
hello
go
$ cat /tmp/dat2
some
writes
buffered
```

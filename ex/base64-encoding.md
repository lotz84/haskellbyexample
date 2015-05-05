```haskell
{-# LANGUAGE OverloadedStrings #-}
import Data.ByteString.Base64

main = do
    let dat = "abc123!?$*&()'-=@~"
    let sEnc = encode dat
    print sEnc

    let sDec = decode sEnc
    print sDec
```

```bash
$ runhaskell base64-encoding.hs
"YWJjMTIzIT8kKiYoKSctPUB+"
Right "abc123!?$*&()'-=@~"
```

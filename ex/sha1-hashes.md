```haskell
{-# LANGUAGE OverloadedStrings #-}
import Numeric (showHex)
import qualified Data.ByteString as BS
import qualified Crypto.Hash.SHA1 as SHA1

main = do
    let s = "sha1 this string"
    let bs = SHA1.hash s

    print s
    putStrLn $ concatMap (flip showHex "") $ BS.unpack bs
```

```bash
$ runhaskell sha1-hashes.hs
"sha1 this string"
cf23df227d99a74fbe169e3eba035e633b65d94
```

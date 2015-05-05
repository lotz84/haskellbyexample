To work this example, you need install [aeson](https://hackage.haskell.org/package/aeson).

```bash
$ cabal install aeson
```

```haskell
{-# LANGUAGE DeriveGeneric, OverloadedStrings #-}
import GHC.Generics
import Data.Aeson
import Data.HashMap.Strict ((!))
import Data.Vector ((!?))
import qualified Data.Map as Map
import qualified Data.ByteString.Lazy.Char8 as BS

data Response1 = Response1 { page :: Int
                           , fruits :: [String]
                           } deriving (Show, Generic)
instance FromJSON Response1
instance ToJSON Response1

main = do
    BS.putStrLn $ encode True
    BS.putStrLn $ encode (1 :: Int)
    BS.putStrLn $ encode (2.34 :: Double)
    BS.putStrLn $ encode ("haskell" :: String)
    BS.putStrLn $ encode (["apple", "peach", "pear"] :: [String])
    BS.putStrLn $ encode $ Map.fromList ([("apple", 5), ("lettuce", 7)] :: [(String, Int)])
    BS.putStrLn $ encode $ Response1 {page = 1, fruits = ["apple", "peach", "pear"]}

    let byt = "{\"num\":6.13,\"strs\":[\"a\",\"b\"]}"
    let dat = decode byt :: Maybe Value
    print dat
    let num = (\(Just (Object map)) -> map ! "num") dat
    print num
    let strs = (\(Just (Object map)) -> map ! "strs") dat
    let str1 = (\(Array vec) -> vec !? 0) strs
    print str1

    let str = "{\"page\": 1, \"fruits\": [\"apple\", \"peach\"]}"
    let Just res = decode str :: Maybe Response1
    print res
    putStrLn $ (fruits res) !! 0
```

```bash
$ runhaskell json.hs
true
1
2.34
"haskell"
["apple","peach","pear"]
{"lettuce":7,"apple":5}
{"fruits":["apple","peach","pear"],"page":1}
Just (Object (fromList [("num",Number 6.13),("strs",Array (fromList [String "a",String "b"]))]))
Number 6.13
Just (String "a")
Response1 {page = 1, fruits = ["apple","peach"]}
apple
```

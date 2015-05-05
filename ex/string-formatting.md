```haskell
{-# LANGUAGE OverloadedStrings #-}
import Data.Char (chr)
import System.IO
import Formatting

data Point = Point { x :: Int
                   , y :: Int
                   } deriving Show

main = do
    let p = Point 1 2

    fprint (shown%"\n") p
    fprint (shown%"\n") True
    fprint (int%"\n") 123
    fprint (bin%"\n") 14
    fprint (char%"\n") $ chr 33
    fprint (hex%"\n") 456
    fprint (float%"\n") 78.9
    fprint ((expt 4)%"\n") 123400000.0
    fprint (text%"\n") "string"
    fprint (shown%"\n") "string"
    fprint ("|"%(left 6 ' ')%"|"%(left 6 ' ')%"|\n") (12 :: Int) (345 :: Int)
    fprint ("|"%(prec 5)%"|"%(prec 5)%"|\n") (1.2 :: Double) (3.45 :: Double)
    fprint ("|"%(left 6 ' ')%"|"%(left 6 ' ')%"|\n") ("foo" :: String) ("b" :: String)
    fprint ("|"%(right 6 ' ')%"|"%(right 6 ' ')%"|\n") ("foo" :: String) ("b" :: String)

    let s = format ("a "%string) "string"
    print s

    hPrint stderr $ format ("an "%string) "error"
```

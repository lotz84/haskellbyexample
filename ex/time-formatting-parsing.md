```haskell
import Data.Time
import System.Locale

main = do
    let rfc3339like = "%FT%T%z"

    t <- getZonedTime
    putStrLn $ formatTime defaultTimeLocale rfc3339like t

    print $ (parseTime defaultTimeLocale rfc3339like "2012-11-01T22:08:41+00:00" :: Maybe ZonedTime)

    putStrLn $ formatTime defaultTimeLocale "%-l:%M%p" t
    putStrLn $ formatTime defaultTimeLocale "%a %b %-e %X %Y" t
    putStrLn $ formatTime defaultTimeLocale "%FT%T.%q%z" t

    let form = "%-l %M %p"
    print $ (parseTime defaultTimeLocale form "8 41 PM" :: Maybe UTCTime)

    let ansic = "%a %b %-e %X %Y"
    print $ (parseTime defaultTimeLocale form "8:41PM" :: Maybe UTCTime)
```

```bash
$ runhaskell time-formatting-parsing.hs
2015-05-05T22:33:20+0900
Just 2012-11-01 22:08:41 +0000
10:33PM
Tue May 5 22:33:20 2015
2015-05-05T22:33:20.122951000000+0900
Just 1970-01-01 20:41:00 UTC
Nothing
```

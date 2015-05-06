```haskell
import Data.Array
import Text.Regex
import Text.Regex.Base

main = do
    let mch = matchTest (mkRegex "p([a-z]+)ch") "peach"
    print mch

    let r = mkRegex "p([a-z]+)ch"

    -- MatchString
    print $ matchTest r "peach"

    -- FindString
    putStrLn $ fst . (!0) . head $ matchAllText r "peach punch"

    -- FindStringIndex
    print $ snd . (!0) . head $ matchAllText r "peach punch"

    -- FindStringSubmatch
    print $ map fst . elems . head $ matchAllText r "peach punch"

    -- FindStringSubmatchIndex
    print $ map snd . elems . head $ matchAllText r "peach punch"

    -- FindAllString
    print $ map (fst . head . elems) $ matchAllText r "peach punch pinch"

    -- FindAllStringSubmatchIndex
    print $ map (map snd . elems) $ matchAllText r "peach punch pinch"

    -- FindAllString (2)
    print $ take 2 . map (fst . head . elems) $ matchAllText r "peach punch pinch"

    -- ReplaceAllString
    putStrLn $ subRegex r "a peach" "<fruit>"
```

```bash
$ runhaskell regular-expressions.hs
True
True
peach
(0,5)
["peach","ea"]
[(0,5),(1,2)]
["peach","punch","pinch"]
[[(0,5),(1,2)],[(6,5),(7,2)],[(12,5),(13,2)]]
["peach","punch"]
a <fruit>
```

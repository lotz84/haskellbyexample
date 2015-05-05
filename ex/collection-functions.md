```haskell
import Data.List
import Data.Char

main = do
    let strs = ["peach", "apple", "pear", "plum"]

    print $ lookup "pear" (zip strs [0..])
    print $ elem "grape" strs
    print $ any (isPrefixOf "p") strs
    print $ all (isPrefixOf "p") strs
    print $ filter (elem 'e') strs
    print $ map (map toUpper) strs
```

```bash
$ runhaskell collection-functions.hs
Just 2
False
True
False
["peach", "apple", "pear"]
["PEACH","APPLE","PEAR","PLUM"]
```

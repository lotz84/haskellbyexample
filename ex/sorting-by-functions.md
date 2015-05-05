```haskell
import Data.List
import Data.Function

main = do
    let fruits = ["peach", "banana", "kiwi"]
    print $ sortBy (compare `on` length) fruits
```

```bash
$ runhaskell sorting-by-functions.hs
["kiwi","peach","banana"]
```

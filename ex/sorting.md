```haskell
import Data.List

main = do
    let strs = "cab"
    putStrLn $ "Strings: " ++ sort strs

    let ints = [7, 2, 4]
    putStrLn $ "Ints: " ++ show (sort ints)

    let s = ints == sort ints
    putStrLn $ "Sorted: " ++ show s
```

```bash
runhaskell sorting.hs
Strings: abc
Ints: [2,4,7]
Sorted: False
```

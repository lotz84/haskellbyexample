```haskell
import Control.Monad
import Data.Map (Map)
import qualified Data.Map as Map

main = do
    let nums = [2, 3, 4]
    putStrLn $ "sum: " ++ show (sum nums)

    mapM_ putStrLn ["index: " ++ show i | (i, num) <- zip [0..] nums, num == 3]

    let kvs = Map.fromList [("a", "apple"), ("b", "banana")]
    forM_ (Map.toList kvs) $ \(k, v) -> putStrLn $ k ++ " -> " ++ v

    mapM_ print $ zip [0..] "haskell"
```

```bash
$ runhaskell range.hs
sum: 9
index: 1
a -> apple
b -> banana
(0,'h')
(1,'a')
(2,'s')
(3,'k')
(4,'e')
(5,'l')
(6,'l')
```

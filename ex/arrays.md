```haskell
import Data.Array

main = do
    let a = array (0, 4) [(i, 0) | i <- [0..4]]
    putStrLn $ "emp: " ++ show a

    let a' = a // [(4,100)]
    putStrLn $ "set: " ++ show a'
    putStrLn $ "get: " ++ show (a' ! 4)
    putStrLn $ "len: " ++ show ((+1) . snd . bounds $ a')

    let b = array (0, 4) [(i, i+1) | i <- [0..4]]
    putStrLn $ "dcl: " ++ show b

    let twoD = array ((0,0), (1, 2)) [((i, j), i + j) | i <- [0..1], j <- [0..2]]
    putStrLn $ "2d: " ++ show twoD
```

```bash
$ runhaskell arrays.hs
emp: array (0,4) [(0,0),(1,0),(2,0),(3,0),(4,0)]
set: array (0,4) [(0,0),(1,0),(2,0),(3,0),(4,100)]
get: 100
len: 5
dcl: array (0,4) [(0,1),(1,2),(2,3),(3,4),(4,5)]
2d: array ((0,0),(1,2)) [((0,0),0),((0,1),1),((0,2),2),((1,0),1),((1,1),2),((1,2),3)]
```

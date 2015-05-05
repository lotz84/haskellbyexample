About Lists

```haskell
main = do
    let s = [] :: [String]
    putStrLn $ "emp: " ++ show s

    let s' = ["a", "b", "c"]
    putStrLn $ "set: " ++ show s'
    putStrLn $ "get: " ++ s' !! 2
    putStrLn $ "len: " ++ show (length s')

    let s2 = s' ++ ["d"]
    let s3 = s2 ++ ["d", "f"]
    putStrLn $ "apd: " ++ show s3

    let c = s3
    putStrLn $ "cpy: " ++ show c

    let l1 = drop 2 . take 5 $ s3
    putStrLn $ "sl1: " ++ show l1

    let l2 = take 5 s3
    putStrLn $ "sl2: " ++ show l2

    let l3 = drop 2 s3
    putStrLn $ "sl3: " ++ show l3

    let t = ["g", "h", "i"]
    putStrLn $ "dcl: " ++ show t

    let twoD = [[i + j | j <- [0..i]] | i <- [0..2]]
    putStrLn $ "2d: " ++ show twoD
```

```bash
$ runhaskell slices.hs
emp: []
set: ["a","b","c"]
get: c
len: 3
apd: ["a","b","c","d","d","f"]
cpy: ["a","b","c","d","d","f"]
sl1: ["c","d","d"]
sl2: ["a","b","c","d","d"]
sl3: ["c","d","d","f"]
dcl: ["g","h","i"]
2d: [[0],[1,2],[2,3,4]]
```

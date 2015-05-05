```haskell
import System.Random

main = do
    putStr . show =<< randomRIO (0, 100 :: Int)
    putStr ", "
    print         =<< randomRIO (0, 100 :: Int)

    print =<< (randomIO :: IO Float)

    f1 <- randomIO :: IO Float
    putStr $ show (f1 * 5 + 5) ++ ", "
    f2 <- randomIO :: IO Float
    print  $ f2 * 5 + 5

    let s1 = mkStdGen 42
    let (i1, s2) = randomR (0, 100 :: Int) s1
    let (i2,  _) = randomR (0, 100 :: Int) s2
    putStrLn $ show i1 ++ ", " ++ show i2

    let s3 = mkStdGen 42
    let (i3, s4) = randomR (0, 100 :: Int) s3
    let (i4,  _) = randomR (0, 100 :: Int) s4
    putStrLn $ show i3 ++ ", " ++ show i4
```

```bash
$ runhaskell random-numbers.hs
51, 15
0.2895795
9.823023, 5.912962
77, 38
77, 38
```

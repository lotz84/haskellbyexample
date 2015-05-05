Type Classes and Class Methods.

```haskell
class Geometry g where
    area :: g -> Double
    perim :: g -> Double

data Square = Square Double Double deriving Show
data Circle = Circle Double deriving Show

instance Geometry Square where
    area (Square w h)  = w * h
    perim (Square w h) = 2 * w + 2 * h

instance Geometry Circle where
    area (Circle r) = pi * r * r
    perim (Circle r) = 2 * pi * r

measure :: (Geometry a, Show a) => a -> IO ()
measure g = do
    print g
    print $ area g
    print $ perim g

main = do
    let s = Square 3 4
    let c = Circle 5

    measure s
    measure c
```

```bash
$ runhaskell interfaces.hs
Square 3.0 4.0
12.0
14.0
Circle 5.0
78.53981633974483
31.41592653589793
```

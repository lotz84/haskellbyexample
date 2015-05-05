Either failure or success.

```haskell
import Control.Monad

f1 :: Int -> Either String Int
f1 arg = if arg == 42
             then Left "can't work with 42"
             else Right (arg + 3)

data ArgError = ArgError Int String

instance Show ArgError where
    show (ArgError arg err) = show arg ++ " - " ++ err

f2 :: Int -> Either ArgError Int
f2 arg = if arg == 42
             then Left (ArgError arg "can't work with it")
             else Right (arg + 3)

main = do
    forM_ [7, 42] $ \i -> do
        case f1 i of
            Left error -> putStrLn $ "f1 failed: " ++ error
            Right value -> putStrLn $ "f1 worked: " ++ show value

    forM_ [7, 42] $ \i -> do
        case f2 i of
            Left err -> putStrLn $ "f2 failed: " ++ show err
            Right value -> putStrLn $ "f2 worked: " ++ show value
    case f2 42 of
        Left (ArgError arg err) -> do
            print arg
            putStrLn err
        _ -> return ()
```

```bash
$ runhaskell errors.hs
f1 worked: 10
f1 failed: can't work with 42
f2 worked: 10
f2 failed: 42 - can't work with it
42
can't work with it
```

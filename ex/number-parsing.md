```haskell
main = do
    print (read "1.234" :: Double)
    print (read "123"   :: Int)
    print (read "0x1c8" :: Int)
    print (read "wat"   :: Int)
```

```bash
$ runhaskell number-parsing.hs
1.234
123
456
number-parsing.hs: Prelude.read: no parse
```

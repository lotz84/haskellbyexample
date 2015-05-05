```haskell
import Data.Char

main = interact $ fmap toUpper
```

```bash
$ echo 'hello'   > /tmp/lines
$ echo 'filter' >> /tmp/lines
$ cat /tmp/lines | runhaskell line-filters.hs
HELLO
FILTER
```

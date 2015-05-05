```haskell
import Network.URI

main = do
    let s = "postgres://user:pass@host.com:5432/path?k=v#f"
    case parseURI s of
        Nothing  -> error "no URI"
        Just uri -> do
            putStrLn $ uriScheme uri
            case uriAuthority uri of
                Nothing   -> error "no Authority"
                Just auth -> do
                    putStrLn $ uriUserInfo auth
                    putStrLn $ uriRegName auth
                    putStrLn $ uriPort auth
            putStrLn $ uriPath uri
            putStrLn $ uriFragment uri
            putStrLn $ uriQuery uri
```

```bash
$ runhaskell url-parsing.hs
postgres:
user:pass@
host.com
:5432
/path
#f
?k=v
```

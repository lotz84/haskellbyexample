```haskell
import Data.Time
import Data.Time.Calendar.WeekDate

main = do
    let i = 2
    putStr $ "write " ++ show i ++ " as "
    case i of
        1 -> putStrLn "one"
        2 -> putStrLn "two"
        3 -> putStrLn "three"
  
    now <- getCurrentTime
    let (_, _, week) = toWeekDate . utctDay $ now
    putStrLn $
        case week of
            6 -> "it's the weekend"
            7 -> "it's the weekend"
            _ -> "it's a weekday"
  
    localtime <- utcToLocalZonedTime now
    let hour = todHour . localTimeOfDay . zonedTimeToLocalTime $ localtime
    case hour of
        _
            | hour < 12 -> putStrLn "it's before noon"
            | otherwise -> putStrLn "it's after noon"
```

```bash
$ runhaskell switch.hs
write 2 as two
it's a weekday
it's after noon
```

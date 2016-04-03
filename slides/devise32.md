こうすれば簡単に!

```csharp
// こっちをテストする
public static bool GetIsInRange(DateTime start, DateTime end, DateTime now)
{
    return start <= now && now < end;
}

public bool IsInRange
{
    get {
        return GetIsInRange(start, end, now);
    }
}
```

クラス全体で使いたいときはこんな感じで!

```csharp
// 普通のコンストラクタ
public Foo():this(() => DateTime.Now) {}

// テスト時のみに使うコンストラクタ
public Foo(Func<DateTime> getNow)
{
    _getNow = getNow;
}

public bool IsInRange
{
    get {
        var now = _getNow();
        return StartDateTime <= now && now < EndDateTime;
    }
}
```

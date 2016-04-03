### 一番単純なパターン

```csharp
// 加算メソッド
public static class Calc
{
    public static int Add(int x, int y)
    {
        return x + y;
    }
}
```

```csharp
[TestFixture]
public class CalcTest
{
    [Test]
    public void TestAdd()
    {
        // 1 + 2 を計算して 3 なのを確認!
        Assert.That(Calc.Add(1, 2), Is.EqualTo(3));
    }
}
```

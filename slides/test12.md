### そして単体テスト!

```csharp
[Test]
public void TestCalculate()
{
    var result = [Default].Calculate(VideoType.New, 8);

    Assert.That(result, Is.EqualTo(4000));
}
```

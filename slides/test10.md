### コードの加工

加工して、テスト対象部分から依存を排除!

```csharp
int result;

switch (type)
{
    case VideoType.Normal:
        if (number <= 7)
        {
            result = 7 * 500 + (number - 7) * 300;
        }

    // ... 省略

    default:
        throw new InvalidOperationException();
}

Price.Text = result.ToString();
```

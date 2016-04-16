### 一時変数の利用

一時変数を利用してフォーム依存部分を排除

```csharp
// レンタル料金を保持する一時変数
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

### メソッドの抽出

```csharp
public static int CalcFee(VideoType type, int number)
    switch (type)
    {
        case VideoType.Normal:
            if (number <= 7)
            {
                return 7 * 500 + (number - 7) * 300;
            }

            // ...
}
```

```csharp
protected void Calculate_Click(object sender, EventArgs e)
{
    // ... 省略

    Price.Text = CalcFee(type, number);

    // ... 省略
}
```

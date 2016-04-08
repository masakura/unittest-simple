### コードの加工

* 今回のソースコードは 3 と 4 が混ざっているので、分離
* ここはテストコードのご加護がないので慎重に

```csharp
switch (type)
{
    case VideoType.Normal:
        if (number <= 7)
        {
            Price.Text = (7*500 + (number - 7)*300).ToString();
        }
        else
        {
            Price.Text = (number*500).ToString();
        }
        break;
```

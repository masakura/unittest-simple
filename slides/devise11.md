抽出したメソッドを新しいクラスに移動するのがおすすめ

```csharp
public static class RentalCalc
{
    public static int CalcFee(VideoType type, int days)
    {
        // ...
    }
}
```

* 再利用性が上がる
* レンタル料金の計算とか外でも使う

# 草案


## 工夫
こういうコードをテストすることを考える。

```csharp
private void ClickCalculateButton(object sender, EventArgs e)
{
	// フォームから選択したビデオを取得

	// データベースからビデオの種別を取得

	// レンタル料金の計算

	// レンタル料金をフォームに表示
}
```

こういうことをしておかないといけない。

* ビデオの情報を格納したデータベースをあらかじめ用意する
* フォームをインスタンス化して、ビデオを選択して、計算ボタンをクリックする
* 結果をフォームから読み取って正常かどうかを判断する

これは結構面倒。

* データベースを予め用意するのは大変
  - 一つのデータベースだけならいいけど、テストが複数になるとかなり厄介
  - テストの速度が遅くなる
  - テストのメンテナンス性が悪化する
* Windows.Forms の場合はフォームの操作が簡単だけど、ASP.NET とかになると...
  - Windows.Forms でもデスクトップが必要になるので、CI が使いづらくなる...

これらの問題に立ち向かうのはかなり大変! チャレンジするのはいいけど、途中でめげて廃墟になったり...

ところがこうすると、簡単にテストできるようになる。

```
public static int CalculateRentalFee(int videoType, int days)
{
	// レンタル料金の計算
}

private void ClickCalculateButton(object sender, EventArgs e)
{
	// フォームから選択したビデオを取得

	// データベースからビデオの種別を取得

	var fee = CalculateRentalFee(videoType, days);

	// レンタル料金をフォームに表示
}
```

もちろん、全体をテストするのは難しいままだけど、`CalculateRentalFee` 関数のテストならとても簡単!

よくある単体テストコードの書き方で最初に紹介されている例だけでテストできるようになる。

こんな感じで、既存のコードをどうやってテストをするか? を考えるのではなく、どうやってテストをする部分を増やすか? を考え、テストを行いやすい部分を見つけ、リファクタリングを行うといい感じになる。


### 副次的効果
副次的な効果として、ややこしい計算の実装を後回しにできるというものがある。

ひとまず、計算用の関数はこんな感じで書いておく。

```
public static int CalculateRentalFee(int videoType, int days)
{
    // ToDo 計算をきちんとする!
	return 300 * days;
}
```

それで、アプリを実際に動かし、使いやすさや、ハッピーパスがきちんと通るか確かめる。それが落ち着いてから、単体テストを利用して料金計算を実装する。

何が起きたかというと...

* 使いやすさの確認やハッピーパスの確認などは人が手でやったほうが早いことが多い
  - そんなに回数行わないし...
* 料金計算などの分岐や計算が多発するところはバグが起きやすく人の手ではテストしにくいところを自動テストに任せる

この、得意な方に任せるということで、早めにアプリの挙動を確かめることができ、テスト自動化による開発速度の低下を起こしにくい。(逆に早くなることもあります)

これを少しずつ続けていくと、自動テストの勘所をつかむことができるので、それに合わせて少しずつ自動化の範囲を広げていくとよい。


### 小技

#### 時刻が絡むテスト
こういう、現在の時刻が絡むところはテストしにくい。

```csharp
public bool IsOutOf
{
    get {
        return EndDateTime < DateTime.Now;
    }
}
```

こうすると簡単になる!

```csharp
public static bool GetIsOutOf(DateTime now, DateTime endDateTime)
{
    return endDateTime < now;
}

public bool IsOutOf
{
    get {
        return GetIsOutOf(DateTime.Now, EndDateTime);
    }
}
```

何にも依存していない static メソッドは本当にテストしやすい! (やり過ぎると読みにくくなるけど..)

オブジェクトに渡す場合はこんな感じで。

```csharp
class Entity
{
    public Entity():this(() => DateTime.Now)
    {
    }

    public Entity(Func<DateTime> getNow)
    {
        _getNow = getNow;
    }
}
```


#### ファイルを読み込むテスト
これはテストだけの話じゃないけど、ファイルを読み込むメソッドはストリームも受け取るようにしたほうがよい。

```csharp
public void ReadConfig() {
    // 設定ファイルを読み込んで...

    // 設定する
}
```

こういうのも...

```csharp
public static AppConfig LoadConfig(TextReader reader)
{
    // 
}

public void ReadConfig() {
    // 設定ファイルを読み込んで...

    // 設定する
}
```


#### キャッシュが使われたかのテストも...
まあ、これはデザインパターンを使うともっとシンプルになるけどね!


#### 長いメソッドも...
長いメソッドは責任ごとに分割して、自身は大雑把なテストにとどめて、細かいところは委譲した先で行うとうまく行くことがある。(うまく行かないとしたら、責任の分割に失敗している)


#### .NET Framework のフレンドアセンブリ


## 参考資料
* [新装版 リファクタリング](http://www.amazon.co.jp/dp/427405019X)

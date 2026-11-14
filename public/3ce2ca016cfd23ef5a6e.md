---
title: 【Moq】関数内の処理が実行されたことを検査するテスト
tags:
  - C#
  - Moq
private: false
updated_at: '2021-12-18T20:42:42+09:00'
id: 3ce2ca016cfd23ef5a6e
organization_url_name: null
slide: false
ignorePublish: false
posting_campaign_uuid: null
agreed_posting_campaign_term: false
---
# 関数内の処理が実行されたことを検査する  
Excelへの描画や画像描画など、関数の結果を直接検査することが難しい時のテスト手法です。

関数の実行は[Moqライブラリ](https://github.com/moq/moq4)の[Verify](https://github.com/moq/moq4/blob/b7b07279c45c29a201399a7fa93764892422e687/src/Moq/ISetup.cs#L114)を用いて確認することができます。
下記のコードにて、Broker.Sell()内でartが思ったとおりに作成されることを検査したいとする。

```csharp
public class Broker
{
	public void Sell()
	{
		// artを作らせる
		var canvas = new Bitmap(10, 100);
		var inspiration = "台北";
		var art = new Calligrapher().Draw(canvas, inspiration, 1, 1);

		// artを売る
		new Buyer().Buy(art);
	}
}

public class Calligrapher : IArtist
{
	public Bitmap Draw(Bitmap canvas, string inspiration, int x, int y)
	{
		// Bitmapにstringを描画する
		using var g = Graphics.FromImage(canvas);
		g.DrawString(inspiration, new Font("Arial", 12), Brushes.Blue, new Point(x, y));

		return canvas;
	}
}
```

Sell()の返り値がわかればそれを検査したいのだが、void型の場合は関数内部で実行されたことを確認する他ない。
テストは下記のようになる。

```csharp
[Test]
public void Test()
{
	// 実行を確認したい座標をSetup
	var mock = new Mock<IArtist>();
	mock
	.Setup(x => x.Draw(It.IsAny<Bitmap>, It.IsAny<string>, 1, 1))
	.Verifiable();

	var canvas = new Bitmap(10, 100);
	var inspiration = "テスト";
	new Calligrapher(mock.Object).Draw(canvas, inspiration);

	// 実態クラスで検査されたか確認
	mock.Verify();
}
```
こうすることでBroker.Sell()内部でCalligrapher.Draw()が（Setupした値で）実行されたことが確認できる。
もしSetupで設定した値で実行されない場合はmock.Verify()で例外がスローされる。



# 組み込みクラスの場合

下記コードで正しくBitmapが生成されたことをテストしたい。
Bitmapをピクセル単位で見ることなく実行されたことを確かめる。

```csharp
public class Calligrapher
{
	public Bitmap Draw(Bitmap canvas, string inspiration)
	{
		// Bitmapにstringを描画する
		using var g = Graphics.FromImage(canvas);
		g.DrawString(inspiration, new Font("Arial", 12), Brushes.Blue, new Point(1, 1));

		return canvas;
	}
}
```

テストでは入力・出力を確認すればよいので、今回はBitmap上に描画される座標が確かめられれば良い為、Moqでスタブを作って確かめる。
しかし現行コードだとDraw()の入出力はテスト側でスタブを作れるが描画する座標は確認できない。

```csharp
// 検査したい組み込みクラスをIFとして公開する
public class GraphicsRapper : IGgraphicsRap
{
	public void DrawString(Bitmap canvas, string char, Font font, Brushes color, Point point)
	{
		using var g = Graphics.FromImage(canvas);
		g.DrawString(char, font, color, point);
	}
}

public class Calligrapher
{
	private IGgraphicsRap _graphicsRapper

	// コンストラクタで入れる
	public Calligrapher(IGgraphicsRap graphicsRapper)
	{
		_graphicsRapper = graphicsRapper;
	}

	public Bitmap Draw(Bitmap canvas, string inspiration)
	{
		_graphicsRapper.DrawString(canvas, char, font, color, point);
		return canvas;
	}
}

[Test]
public void Test()
{
	//　検査したい条件をSetup. 検査したいの座標なので座標以外はIt.IsAny<>で型だけ指定する.
	var mock = new Mock<IGraphicsRap>();
	mock
	.Setup(x => x.DrawStinrg(It.IsAny<Bitmap>, It.IsAny<string>, It.IsAny<Font>, It.IsAny<Brushes>, new Point(1, 1)))
	.Verifiable();

	new Calligrapher(mock.Object).Draw("テスト");

	// 実態クラスで検査されたか確認
	mock.Verify();
}
```

こうすることでBitmpのようなIFのない組み込みクラスでも実行されたか検査することができる.
IFさえ作ってしまえば検査することは可能になる。

注意点としては、テストがメソッドの内部実装に依存してしまっているので実装を変更したらテストも修正しないとならない可能性があります。

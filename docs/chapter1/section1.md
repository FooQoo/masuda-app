# プログラムの変更が楽になる書き方

- 長いメソッドと大きなクラスを減らす
- どこに何が書いてあるのかわかりやすくする

## わかりやすい名前を使う

業務で使っている言葉をそのままプログラム要素の名前に使うことで、プログラムの変更が楽で安全になる。

悪い名前

| 1文字の変数名 | 省略した変数名 |
|-------|--------|
| int a; | int qty; | 
| int b; | int ut; | 

良い名前

| 意味のある単語を使った変数名 | 
|-------|
| int quantity; |
| int unitPrice; |

## 長いメソッドは空白行をいれて読みやすくする

前後の行と意味が異なる箇所に、空白行を追加することで、変更の対照範囲が特定しやすくなる。

悪いコード

```
int price = quantity * unitPrice;
if(price< 3000)
    price += 500;
price = price * taxRate();
```

良いコード

```
int price = quantity * unitPrice;

if(price< 3000)
    price += 500;
    
price = price * taxRate();
```

## 目的ごとに変数を用意する

目的ごとに変数を用意することで、段落の結びつきが弱くなり、変更の影響範囲を少なくできる。

また、各ステップでの計算の目的が明確になる。

```
final int basePrice = quantity * unitPrice;

final int shippingCost = basePrice < 3000 ? 500 : 0;

final int itemPrice = (basePrice + shippingCost) * taxRate();
```

## メソッドとして独立させる

メソッドとして独立させることで、 3点のメリットがある。

- 元のコードがよりシンプルになり、読みやすい
- メソッド名からコードの意図を理解しやすい
- 変更の影響をメソッド内に閉じ込められる

```
final int basePrice = quantity * unitPrice;

final int shippingCost = shippingCost(basePrice);

final int itemPrice = (basePrice + shippingCost) * taxRate();

int shippingCost(final int basePrice) {
    return basePrice < 3000 ? 500 : 0;
}
```

## 異なるクラスの重複したコードをなくす

共通のロジックの置き場所として、個別のクラスを作成することで、コードの重複をなくせる。

以下の例では、送料計算ロジックを個別のクラスとして作成した。

各商品クラスごとに、送料計算ロジックのメソッドを作成するより、単一のクラスとすることで、変更対応の影響範囲を小さくできる。

```
@RequiredArgsConstructor
class ShippingCost {

    private final static int MINIMUM_FOR_FREE = 3000;
    private final static int COST = 300;
    
    private final int basePrice;
    
    public int amount() {
        return basePrice < MINIMUM_FOR_FREE ? COST : 0;
    }
}
```

## 狭い関心ごとに特化したクラスにする

特定の関心ごとに特化した小さなクラスを作成することで、プログラム全体から関心ごとの記述をみつけることが簡単になる。

業務で使われる用語に合わせて、用語の関心ごとに対応するクラスを「ドメインオブジェクト」と呼ぶ。

```
/**
 *
 */
@RequiredArgsConstructor
class ShippingCost {

    ...
    
    public int amount() {
        return basePrice < MINIMUM_FOR_FREE ? COST : 0;
    }
}
```

> オブジェクト指向のキモ
>
> 業務の用語と直接対応するドメインオブジェクトを用意することが、業務アプリケーションの変更を容易にする
>
> 業務を理解するために、要求を分析し、そこで発見した業務の関心ごとの単位をそのままモデル化する
# クラス設計の大切なこと

使う側のクラスのコードがシンプルになるように設計する。

1. メソッドをロジックの置き場所にする
2. ロジックをデータを持つクラスに移動する
3. 使う側のクラスにロジックを書き始めたら、設計を見直す
4. メソッドを短くして、ロジックの移動をやりやすくする
5. メソッドでは、必ずインスタンス変数を使う
6. クラスが肥大化したら、小さく分ける
7. パッケージを使ってクラスを整理する

## メソッドをロジックの置き場所にする

インスタンス変数を返すだけのgetterを書かない。

自分のデータをそのまま別のクラスに渡してしまうから、ロジックがメソッド外に発生してしまう。

## ロジックをデータを持つクラスに移動する

getterメソッドを何らかの判断/加工/計算をさせるメソッドに変更する。

## 使う側のクラスにロジックを書き始めたら、設計を見直す

最初からいい設計が見つかることはないので、TODOなどでコメントに残す。

改善を通して、よりよい設計にしていく。

## メソッドを短くして、ロジックの移動をやりやすくする

メソッドは小さく分けて、独立させる。

これにより、クラスに相応しくないコードの塊を発見しやすくなる。

## メソッドでは、必ずインスタンス変数を使う

インスタンス変数を使わないメソッドは、クラスに置く意味がないので、相応しいメソッドへの移動を検討する。

## クラスが肥大化したら、小さく分ける

インスタンス変数が多いクラスに業務ロジックを書いていくと、どこに何が書いてあるかわからなくなる。

関連性の強いデータとロジックを抜き出して、新しいクラスに分ける。

## パッケージを使ってクラスを整理する

関連性の強いクラスは同じパッケージに集め、クラスやメソッドのスコープは可能な限りパッケージスコープにする。

public宣言をしないように意識することで、影響範囲をパッケージに閉じ込めやすくなる。


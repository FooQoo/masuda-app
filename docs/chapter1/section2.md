# 小さなクラスでわかりやすく安全に

## 値オブジェクト(Value Object)

以下のポイントを踏まえて、値を扱うための専用のクラスを作る。

- 業務の用語そのものと一致させること
- 値オブジェクトは不変(immutable)にする。

> 専用の型を作るメリット
>
> ・業務的に不適切な値が混入するバグを防ぐ
>
> ・クラス名などを手がかりに変更の対象箇所が特定しやすくなる
>
> ・コードの意図が明確になる
>
> ・値オブジェクトを不変にすることで、変更によるバグを減らす



# プログラムを複雑にする場合分けのコード

場合分けのコードの整理方法を紹介する。

## 判断や処理のロジックをメソッドに独立させる

メソッドに独立させることで、もともとのif文がシンプルな表現になり、意図が明確になる。

```
final int fee = isChild() ? childFee() : baseFee;

private boolean isChild() {
    return "child".equals(customerType);
}

private int childFee() {
    retunr baseFee * 0.5;
}
```

## ガード節を利用する

ローカル変数とelse句をなくし、早期リターンする書き方。

if-then-elseだと複文になるが、単文にできるため、ロジックごとの独立性が高まる。

これにより、変更が容易になり、意図をわかりやすくできる。

```
if (isChild()) return childFee(); // 早期リターンする
if (isSenior()) return seniorFee(); // 早期リターンする

return adultFee();
```

## 列挙型を使う

列挙型を使って、区分ごとのロジックをわかりやすく整理する方法を、区分オブジェクトと呼ぶ。

```
// 料金区分ごとのロジック
@RequiredArgsConstructor
enum FeeType {
    ADULT("adult", new Fee(100)),
    CHILD("child", new Fee(20)),
    SENIOR("senior", new Fee(50));
    
    private final String feeTypeName;
    
    private final Fee fee;
    
    public Yen yen() {
        return fee.yen();
    }
    
    public FeeType getFeeType(final String feeTypeName) throws IllegalArgumentException {
        return Arrays.stream(FeeType.values())
                .filter(feeType -> feeType.getFeeTypeName.equals(feeTypeName))
                .findFirst().orElseThrow(IllegalArgumentException::new);
    }
}

---
// Feeドメインクラス
@RequiredArgsConstrucor(access = AccessLevel.PRIVATE)
class Fee {
    private final Yen yen;
    
    public static Fee from(final int yen) {
        return new Fee(Yen.from(yen));
    }
    
    public Yen yen() {
        return yen;
    }
}

--- 

// Yen値オブジェクト
@RequiredArgsConstrucor(access = AccessLevel.PRIVATE)
@Getter
class Yen {
    private final Integer yen;
    
    public static Yen from(final int yen) throws InvalidValueExcepiton {
        if ((yen < 0) || (10000 < yen)) throw new InvalidValueExcepiton();
        return new Yen(yen);
    }
}

--- 
// 呼び出し元
Yen feeFor(final String feeTypeName) {
    return FeeType.getFeeType(feeTypeName).yen();
}

```

## 状態の遷移ルールをわかりやすくする

Enumを活用することで、if/switchの構文を利用せずに、遷移可能か判定できる。

Map形式で遷移を表現することで、状態の種類が増えたり、状態遷移の制約が増えても副作用の心配がなくなる。

``` 
// 状態のenumクラス
@RequiredArgsConstructor
enum State {
    REVIEW("審査中"),
    APPROVED("承認済み"),
    REMAND("差し戻し中"),
    IN_PROGRESS("実施中"),
    PENDING("中断"),
    COMPLETED("終了");
    
    private final String name;
}
---

class StateTrasitions {
    private static final Map<State, Set<State>> ALLOWED = Map.of(
        REVIEW, EnumSet.of(APPROVED, REMAND),
        REMAND, EnumSet.of(REVIEW, COMPLETED)
    );
    
    public boolean canTransit(final State from, final State to) {
        return allowed.get(from).contains(to);
    }
}
```
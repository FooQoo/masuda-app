# 複雑さを閉じ込める

## 配列やコレクションのロジックを専用クラス(コレクションオブジェクト)に閉じ込める

クラスのコレクションを一つだけ持った専用クラスを作成する。

データと関連するロジックは一つのクラスに集める。

これにより、ロジックを変更するときの影響をコレクションオブジェクトに閉じ込めやすくなる。

```
@RequiredArgsConstructor
class Customers {
    private final List<Customer> coustomers;
    
    void add(final Customer customer) { ... }
    void removeIfExist(final Customer customer) { ... }
    
    int count() { ... } 
}
```

## コレクションオブジェクトも不変(immutable)にする

値オブジェクトの場合と同じように、できるだけ、コレクションの要素も不変になるようにする。

どうしてもコレクションを返す必要がある場合は、変更不可のコレクションにして、渡す。

```
@RequiredArgsConstructor
class Customers {
    private final List<Customer> coustomers;
    
    public Customers add(final Customer customer) { 
        final List<Customer> result = new ArrayList<>(customer);
        return new Customers(result.add(customer));
    }
    
    public List<Customer> asList() {
        return Collections.unmodifiableList(customers);
    }
}
```


# HashSet vs HashMap

### HashMap\<K, V>

```rust
use std::collections::HashMap;

let mut map = HashMap::new();

map.insert("key1", "value1");  // (키, 값) 쌍으로 저장
map.insert("key2", "value2");

println!("{}", map.get("key1").unwrap());  // "value1" 출력
```

* 키(Key)와 값(Value) 쌍을 저장
* 각 키는 고유해야 함
* 예: {"name": "John", "age": 30}

### HashSet\<T>

```rust
std::collections::HashSet;

let mut set = HashSet::new();

set.insert("value1");  // 값만 저장
set.insert("value2");

println!("{}", set.contains("value1"));  // true 출력
```

* 값만 저장 (키가 없음)
* 중복된 값을 허용하지 않음
* 예: {"apple", "banana", "orange"}

#### 내부 구현 관계

```rust
// 실제 Rust 표준 라이브러리에서 HashSet은 HashMap을 이용해 구현됨
pub struct HashSet<T, ...> {
    pub(crate) map: HashMap<T, (), ...>,  // 값을 키로 사용하고, 값은 빈 튜플
}
```

#### 사용 예시 비교

```rust
// HashMap 사용
let mut scores = HashMap::new();
scores.insert("Alice", 100);
scores.insert("Bob", 95);

// HashSet 사용
let mut unique_names = HashSet::new();
unique_names.insert("Alice");
unique_names.insert("Bob");
unique_names.insert("Alice");  // 무시됨 (이미 존재)
println!("{}", unique_names.len());  // 2 출력 (중복 제거)// Some code
```

#### **공통점**

* O(1) 시간 복잡도로 검색/삽입/삭제 가능
* 해시 함수를 사용해 데이터를 저장
* 순서가 보장되지 않음

#### **주요 용도**

* HashMap: 키-값 관계가 필요할 때 (예: dictionary, cache)
* HashSet: 중복 제거나 멤버십 검사가 필요할 때

# String vs \&str

`String`은 확장 가능, 힙(heep)에 할당되고, UTF-8로 인코딩된 문자열.

Rust 표준 라이브러리의 일부이며, 런타임에 수정 및 크기 조절이 가능한 변경 가능하고 **동적인 문자열**이 필요할 때 널리 사용.

```
let mut my_string = String::new();
my_string.push_str("Hello, ");
my_string.push('R');
my_string.push('u');
my_string.push('s');
my_string.push('t');

println!("{}", my_string); // Output: "Hello, Rust"
```



`&str`은 Rust의 문자열 슬라이스(string slice)를 의미하며, 문자열 데이터를 효율적으로 참조하기 위해 설계된 타입.\
`&str`은 불변(immutable)하며, 일반적으로 문자열 데이터의 **뷰(view)** 역할.

```
let my_name_str = "David";
my_name_str.push('R');

/*
error[E0599]: no method named `push` found for reference `&str` in the current scope
   |
47 |     my_name_str.push('R');
   |                 ^^^^ method not found in `&str`
*/
```

`my_name_str`은 `&str` 타입으로, 불변(immutable)이기 때문에 `.push()`를 호출하려고 하면 컴파일 오류가 발생.



### \&str의 사용 사례

* 문자열 리터럴은 기본적으로 `&str` 타입

```
let s = "Hello, World!";
println!("{}", s);
```



* 슬라이스로 부분 문자열 참조

```
let full = "Hello, World!";
let part: &str = &full[0..5]; // "Hello"
println!("{}", part);
```



* 효율적으로 문자열 전달할 때

```
fn count_chars(s: &str) -> usize {
    s.len()
}

let my_string = "Hello, world!";
let length = count_chars(my_string);
println!("Length: {}", length);
```



* `String` 과의 상호작용

```
let s = String::from("hello");
let slice: &str = &s; // String -> &str 참조
let new_string = slice.to_string(); // &str -> String
```



### 언제 `&str`을 `String`으로 변환하나?

| 상황             | 이유                                |
| -------------- | --------------------------------- |
| 데이터를 수정해야 할 때  | `String`은 가변적이지만 `&str`은 불변.      |
| 소유권이 필요할 때     | 함수 호출/반환 또는 데이터 이동 시 사용.          |
| 크기를 동적으로 변경할 때 | `&str`은 고정 크기, `String`은 유동적.     |
| 데이터 구조와 호환     | `HashMap`, `Vec` 등은 `String`을 사용. |
| I/O 작업         | 파일 및 네트워크 작업은 `String` 필요.        |



### \&str vs String 비교

| 특징       | `&str` (문자열 슬라이스)    | `String`               |
| -------- | -------------------- | ---------------------- |
| 저장 위치    | 스택(stack) 또는 정적 메모리  | 힙(heap)                |
| 크기       | 고정된 크기               | 동적으로 크기 조정 가능          |
| 가변성      | 불변(immutable)        | 기본적으로 불변, `mut`로 가변 가능 |
| 소유권      | 소유권 없음(참조 타입)        | 소유권 있음                 |
| 주요 사용 사례 | 간단하고, 변경되지 않는 문자열 처리 | 동적이고 변경 가능한 문자열 처리     |

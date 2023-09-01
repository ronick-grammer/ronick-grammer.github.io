---
title:  "[Swift] Subscripts"
excerpt: "[Swift] Subscripts"
toc: true
toc_sticky: true

header:
  teaser: /assets/images/bio-photo-keyboard-teaser.jpg

categories:
  - Swift
---

### Subscripts란 
`class`, `struct` 그리고 `enum` 타입에서 배열의 인덱스처럼 데이터의 특정 요소에 접근할 수 있도록 해주는 Swift의 문법이다.

### Subscript의 기본 형태
- `subsscript`는 `computed-property`(연산 프로퍼티) 처럼 `getter`와 `setter`를 지정한다.
```swift
subscript(index: Int) -> Int {
    get {
        // 특정 값을 반환한다.
    }
    set(newValue) {
        //  set action을 수행한다.
    }
}
```

- `read-only` or `read and write` 로 동작한다. (아래 예시는 `read-only`)
```swift
struct TimesTable {
    let multiplier: Int
    subscript(index: Int) -> Int {
        return multiplier * index
    }
}
let threeTimesTable = TimesTable(multiplier: 3)
print("six times three is \(threeTimesTable[6])")
```

### Array와 Dictionary
Swift의 데이터 타입인 `Array`와 `Dictionary`도 인덱스를 통해 값을 할당/접근 할때, Subscript를 사용한다는 것을 유추해볼 수 있다.

- **Array**

```swift
let nums: [Int] = [1, 2, 3, 4]
 
nums[0]                // 1
nums[1]                // 2
```

- **Dictionary**

```swift
let dict: [String: Int] = ["one": 1, "two": 2]
 
dict["one"]             // 1
dict["two"]             // 2
```

### Subscript 옵션
Subscript는  일반적인 메서드처럼 제공하는 옵션들이 있다.

- Subscript는 여러 파라미터를 받을 수 있다.
- Subscript의 파라미터들의 데이터 타입은 제한이 없다.
- Subscript의 파라미터들은 Default Value를 설정할 수 있다.
- Subscript는 overloading 이 가능하다.

```swift
struct Matrix {
    let rows: Int, columns: Int
    var grid: [Double]
    init(rows: Int, columns: Int) {
        self.rows = rows
        self.columns = columns
        grid = Array(repeating: 0.0, count: rows * columns)
    }
    
    func indexIsValid(row: Int, column: Int) -> Bool {
        return row >= 0 && row < rows && column >= 0 && column < columns
    }
    
    subscript(row: Int, column: Int) -> Double {
        get {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            return grid[(row * columns) + column]
        }
        set {
            assert(indexIsValid(row: row, column: column), "Index out of range")
            grid[(row * columns) + column] = newValue
        }
    }
    
    // 함수 오버로딩 with Default Value
    subscript(exampleIndex: Int = 1) -> Int {
        return exampleIndex + 2
    }
}

var matrix = Matrix(rows: 2, columns: 2)

matrix[0, 1] = 1.5
matrix[1, 0] = 3.2
matrix[]     // "3"
matrix[3]   // "5"

```

### 📝 참고 사이트
- [Subscripts](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/subscripts/)
- [Swift) 서브스크립트(Subscript) 정복하기](https://babbab2.tistory.com/123)
- [오늘의 Swift 상식 (Subscript)](https://medium.com/@jgj455/%EC%98%A4%EB%8A%98%EC%9D%98-swift-%EC%83%81%EC%8B%9D-subscript-2288551588f9)

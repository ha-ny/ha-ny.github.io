---
layout: post
title: "Class & Struct"
author: "Hany"
tags: Swift
---
##### 공식 문서
[Structures and Classes | Documentation (swift.org)](https://docs.swift.org/swift-book/documentation/the-swift-programming-language/classesandstructures/)

## 1.  사용자 정의 타입

개발자가 직접 타입을 구현하고 싶을 때 사용자 정의타입인 class, struct, enum을 사용한다.
Basic type인 Int, String, Bool도 내부적으로 struct를 사용한다는 것을 알 수 있다
```swift
@frozen public struct Int : FixedWidthInteger, SignedInteger { ... }
```

## 2. Class의 초기화 방식

class의 initializing type(초기화 방식)은 초기값 지정과 생성자 생성으로 구분할 수 있다
둘다 하지 않으면 "Class 'SwiftClass' has no initializers" 오류가 뜬다
```swift
class SwiftClass {

    var apple = "사과"
    var bear: String
    var count: Int
}
```

- 초기값 지정  
위 코드와 같이 apple = “사과”와 같은 형태로 값을 초기화하는 것이다.

- 생성자 구현  
생성자는 init() { } 형태로 사용한다. 
파라미터로 값을 받아오기 때문에 SwiftClass의 가로를 열면 bear과 count가 뜬다.

```swift
class SwiftClass {

    var apple = "사과"
    var bear: String
    var count: Int

    init(bear: String, count: Int) {
        self.bear = bear
        self.count = count
    }
}

let data = SwiftClass(bear: String, count: Int)
```
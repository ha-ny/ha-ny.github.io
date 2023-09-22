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

## 3. Struct의 Initializer

Struct는 생성자를 자동으로 만들어주는 기능이 있다. 이를 Memberwise 기능이라고 한다.
아래와 같이 초기값이 있는 apple은 선택, bear와 count은 필수로 값을 담는다.

```swift
struct SwiftStruct {

    var apple = "사과"
    var bear: String
    var count: Int
}

let data = SwiftStruct(bear: String, count: Int)
```

- 사용자가 직접 생성자를 만들면 어떻게 될까?  
Memberwise 기능이 필요 없어지니 개발자가 만든 생성자가 뜬다. (count가 뜨지 않음)

```swift
struct SwiftStruct {

    var apple = "사과"
    var bear: String
    var count: Int?

    init(apple: String, bear: String) {
        self.apple = apple
        self.bear = bear
    }
}

let data = SwiftStruct(apple: String, bear: String)
```

## 4. 상속관계

Class는 상속이 가능하지만 `단일 상속`만 가능하며, Struct는 상속이 불가능하다.
아래의 코드에서 Zero는 One의 부모 객체이다. 반대로 One은 Zero의 자식 객체이다
One을 상속받은 Two도 Zero의 함수를 사용할 수 있는데, 상속의 특징은 수직과 중첩이다.

```swift
class Zero {

    func zeroFunc() { }
}

class One: Zero {

    override func zeroFunc() {
        super.zeroFunc()
    }

    func oneFunc() { }
}

  

class Two: One {

    override func zeroFunc() {
        super.zeroFunc()
    }

    override func oneFunc() {
        super.oneFunc()
    }
}
```

- 정리  
Class는 상속 관계를 가질 수 있다 → 부모는 하나지만 자식은 여럿일 수 있다 → 단일 상속  
Struct는 상속 관계를 가질 수 없다 → 부모도 자식도 안된다 

## 5. Instance

```swift
struct SwiftStruct {

    var apple = "사과"
    var bear: String
    var count: Int?
}

let word = SwiftStruct(bear: "곰", count: 2)
```

SwiftStruct 생성자의 값은 Instance이다.
즉, word의 타입은 SwiftStruct이고 값은 SwiftStruct의 Instance이다!
생성자를 통해 let word, let word2 와 같은 개별적인 값을 가진 인스턴스를 생성할 수 있다.

## 6. Static(전역)

```swift
struct SwiftStruct {

    static var apple: String?
    var bear: String
}

let word = SwiftStruct(bear: String)
```

bear은 SwiftStruct의 생성자에 포함되어 있다. → bear은 인스턴스 프로퍼티이다  
apple은 생성자에 포함되어 있지 않다 → 앞에 static 붙으면 타입 프로퍼티가 된다  

- apple에 접근하려면 어떻게 해야할까?

```swift
struct SwiftStruct {

    static var apple: String?
    var bear: String
}

SwiftStruct.apple
```

Swiftstruct 타입을 참조하면 apple 프로퍼티가 뜬다  
---
layout: post
title: "Class & Struct"
author: "Hany"
tags: Swift
---

## 1. 사용자 정의 타입

개발자가 직접 타입을 구현하고 싶을 때 class, struct, enum을 사용할 수 있다.
class, struct, enum은 사용자 정의타입이다.
Int, String, Bool은 애플이 구현해둔 Basic type이다.
애플이 구현해둔 Basic type인 Int, String, Bool는 내부적으로 struct를 사용한다는 것을 알 수 있다

<img width="474" alt="11" src="https://github.com/ha-ny/ha-ny/assets/130643750/218ac7eb-4497-4784-83d6-6e42d763096b">  

## 2. Class의 초기화 방식

class의 initializing type(초기화 방식)은 초깃값 지정과 생성자 생성으로 구분할 수 있다
둘다 하지 않으면 아래와 같은 오류가 뜬다

<img width="1016" alt="초기화방식" src="https://github.com/ha-ny/ha-ny/assets/130643750/521e432f-b939-4e45-ae17-4bc70529ebd3">

- 초깃값 지정  
위 코드와 같이 apple = “사과”와 같은 형태로 값을 고정하는 것이다.

- 생성자 구현  
만약 count의 값을 계속 변경해주고 싶다면 초깃값 지정보다 생성자(Initializer)를 구현해야한다.
생성자는 init() { } 형태로 사용한다. 

<img width="512" alt="7" src="https://github.com/ha-ny/ha-ny/assets/130643750/6ec57d21-4aa6-4dd7-bc06-e526a5614d02">

SwiftClass의 가로를 열면 bear과 count가 뜨는데 이는 생성자의 파라미터이다.
물론 초깃값을 지정해 준 apple도 생성자의 파라미터로 추가할 수 있다.
외부에서 값을 받아올 건지 / 지정된 값을 사용할 건지의 차이에 따라 추가하면 된다.

## 3. Struct의 Initializer

Struct는 생성자를 자동으로 만들어주는 기능이 있다. 이를 Memberwise 기능이라고 한다.
아래와 같이 초깃값이 있는 apple은 선택적으로 값을 넘겨줄 수 있고, bear와 count은 기본적으로 값을 넘겨줘야 한다.

<img width="434" alt="생성자" src="https://github.com/ha-ny/ha-ny/assets/130643750/472c5a6a-5442-4ddd-a420-ed9928cb82f4">

- 사용자가 직접 생성자를 만들면 어떻게 될까?  
Memberwise 기능이 필요 없어지니 개발자가 만든 생성자가 뜬다. ( 가로를 열었을 때 count가 뜨지 않는 것을 보면 알 수 있다)

<img width="434" alt="2" src="https://github.com/ha-ny/ha-ny/assets/130643750/3d8cc55c-68ad-4d3c-87e2-2a7041cb8b55">

## 4. 상속관계

Class는 단일 상속만 가능하며, Struct는 상속이 불가능하다.
아래의 코드에서 zero는 one의 부모 객체이다. 반대로 one은 zero의 자식 객체이다
one을 상속받은 two도 zero의 함수를 사용할 수 있는데, 상속의 특징은 수직과 중첩이다.

<img width="268" alt="8" src="https://github.com/ha-ny/ha-ny/assets/130643750/20c20803-930f-477d-8ec2-aeb322df3f32">

- 정리  
class는 상속 관계를 가질 수 있다 → 부모는 하나지만 자식은 여럿일 수 있다 → 단일 상속  
struct는 상속 관계를 가질 수 없다 → 부모도 자식도 안된다 

## 5. Instance

<img width="342" alt="4" src="https://github.com/ha-ny/ha-ny/assets/130643750/d65610e2-6ce1-4b4a-8775-1787c17f2efa">

SwiftStruct init()의 반환 값은 Instance이다.
반대로, word의 타입은 생성자의 타입인 SwiftStruct이다.
생성자를 통해 let word, let word2 와 같은 개별적인 값을 가진 인스턴스를 생성할 수 있다

## 6. Static(전역)

<img width="519" alt="5" src="https://github.com/ha-ny/ha-ny/assets/130643750/a3d041bd-b8d8-47a4-b4c0-5a184fe29a3c">

bear은 Swiftstruct의 생성자에 포함되어 있다. → bear은 인스턴스 프로퍼티이다  
apple은 생성자에 포함되어 있지 않다 → 앞에 static 붙으면 타입 프로퍼티가 된다  
타입 프로퍼티는 Swiftstruct(사용자 정의 타입)의 변수가 된다

- apple에 접근하려면 어떻게 해야할까?

<img width="358" alt="6" src="https://github.com/ha-ny/ha-ny/assets/130643750/2562ae52-3270-405a-86f6-c4615bd0ec5b">

Swiftstruct 타입을 참조하면 apple 프로퍼티가 뜬다  

타입 프로퍼티와 인스턴스 프로퍼티 / 타입 매서드와 인스턴스 매서드  
인스턴스를 통해 참조하는지, 타입을 통해 참조하는지에 대한 차이라고 생각하면 된다.

## 7. 참조 타입과 값 타입

Class (reference type) =  참조 타입 (주소 값을 참조하고 있다)  
Struct (value type) = 값 타입(값을 저장하고 있다)

<img width="993" alt="9" src="https://github.com/ha-ny/ha-ny/assets/130643750/a77643f8-1fa5-4ffa-a72b-44f89180423a">

- 같은 형태의 코드인데 왜 마지막 줄은 오류가 날까?  
valueS는 let이라서 name값을 바꿀수 없다고 한다. valueC는 let인데 name 값을 바꿀 수 있다.  
class는 참조 타입이라 주소 값을 참조하고 있다. → valueC는 직접적으로 값을 변경하는 게 아닌 주소 값에 있는 데이터를 변경하는 것이다.  
struct는 값 타입이라 값을 저장한다. → valueS는 자신이 저장한 값이 직결되어 있고 그 값이 변경된다.

<img width="412" alt="10" src="https://github.com/ha-ny/ha-ny/assets/130643750/1dc3ce91-e09c-4cb9-a998-838f9c8477cb">

- 같은 형태의 코드인데 왜 값이 다를까?  
class는 참조 타입이라 주소 값을 참조하고 있다. → valueC2는 valueC의 주소 값을 참조했다. valueC2는 valueC와 같은 주소를 바라보고 있기 때문에 변경된 값을 공유된다  
struct는 값 타입이라 값을 저장한다. → valueS2에 valueS의 값을 복사했다. valueS2를 수정해도 valueS의 데이터는 수정되지 않는다.   
위 코드에서 valueS는 let이라서 name 값을 바꿀 수 없었다. 만약 valueS2를 수정 시 valueS의 값이 변경된다면 valueS은 let으로 선언되어 있기 때문에 오류가 났을 것이다

- 참조 타입

![](https://github.com/ha-ny/ha-ny/assets/130643750/7de6086a-7671-417e-be39-1882b7808a67)

- 값 타입

![](https://github.com/ha-ny/ha-ny/assets/130643750/1d499417-6262-4a52-81e3-19ed0247e0de)





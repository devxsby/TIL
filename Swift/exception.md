# 예외처리 (throws, throw, do-try-catch)

## 1. 예외처리가 필요한 이유

옵셔널 타입은 오류가 발생했을 때 오류에 대한 정보를 외부로 전달할 방법이 없다. 따라서 예외처리로 Error를 표현한다.

## 2. 구현 방법

- 함수가 반환할 오류는 일관된 주제와 연관된 경우 (문자열이 있을 때, 문자열 크기, 문자열 형식, 문자열 포맷 형식 등의 체크)

- 일관된 주제를 표현하기에 가장 좋은 것은 `enum`형 타입

### 2-1. enum에 오류타입명 정의

```swift
// enum형으로 error타입명 정의 (프로토콜을 구현한 것은 오류 타입으로 사용하라는 일종의 가독성 표시)
enum DateParseError : Error {
    case overSizeString
    case incorrectData(part: String)
}
```

### 2-2. 오류가 나는 조건을 명시 : throws와 thorw로 선언

- 오류가 발생할 수 있다는 표현 : `throws`키워드 -> (반환type)

- 오류가 발생했다는 표현 : throw와 오류명 표시 (return과 유사)

```swift
// 오류가 나는 조건을 throws와 함께 배치
func getNextYearAndThrows(paramYear: Int) throws -> Int {
    guard paramYear <= 2020 else {
        throw DataError.overSizeYear
    }
    
    guard paramYear >= 0 else {
        throw DataError.incorrectData(part: paramYear)
    }
    
    return paramYear+1
}
```

### 2-3. do - try - catch로 접근

- 마지막 catch에 오류타입을 작성하지 않으면 default문으로 됨

- `try!` : 함수 강제 실행 (런타임 오류 발생 가능)

- `try?` : 에러가 발생하면 nil 반환, 발생하지 않으면 optional 타입으로 반환

```swift
func getNextYear(paramYear: Int) -> Int {
    var year: Int = 0
    do {
        year = try getNextYearAndThrows(paramYear: paramYear)
    } catch DataError.overSizeYear {
        print("년도 초과해서 입력하였습니다")
    } catch DataError.incorrectData(let part){
        print("입력한 값이 \(part)이므로 오류입니다.")
    } catch {
        print("default error catch")
    }
    
    return year
}

let a = getNextYear(paramYear: -999) // 입력한 값이 -999이므로 오류입니다.
```

> **정리**
> 
> 오류가 나는 조건은 따로 함수로 작성 (throws, throw)
> 
> 오류를 처리하는 구문은 직접 함수를 이용하는 곳에서 처리 (do - try - catch)

### 예시 전체 코드

```swift
// enum형으로 error타입명 정의
enum DataError : Error { // Error 프로토콜을 구현한 것은 오류 타입으로 사용하라는 일종의 가독성 표시
    case overSizeYear
    case incorrectData(part: Int)
}

// 오류가 나는 조건을 throws와 함께 배치
func getNextYearAndThrows(paramYear: Int) throws -> Int {
    guard paramYear <= 2020 else {
        throw DataError.overSizeYear
    }
    
    guard paramYear >= 0 else {
        throw DataError.incorrectData(part: paramYear)
    }
    
    return paramYear+1
}

// 실제로 응용프로그램에서 불러올 함수
func getNextYear(paramYear: Int) -> Int {
    var year: Int = 0
    do {
        year = try getNextYearAndThrows(paramYear: paramYear)
    } catch DataError.overSizeYear {
        print("년도 초과해서 입력하였습니다")
    } catch DataError.incorrectData(let part){
        print("입력한 값이 \(part)이므로 오류입니다.")
    } catch {
        print("default error catch")
    }
    
    return year
}

let a = getNextYear(paramYear: -999) // 입력한 값이 -999이므로 오류입니다.​
```

## 3. try, try!, try? 차이

- `try` : 예외 발생시 catch로 예외 정보 던지며, 성공하는 경우 언래핑된 값 반환

- `try!` : 예외 발생시 runtime error, 성공하는 경우 언래핑된 값 반환 (nil값이 나오지 않음을 확신하는 경우 사용)

- `try?` : 예외 발생시 nil 리턴, 성공하는 경우 옵셔널 값 반환


---

### 참고자료

[[swift] 12. 예외 처리 (throws, throw, do - try - catch)](https://ios-development.tistory.com/16?category=887218)

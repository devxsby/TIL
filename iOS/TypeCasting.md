# 타입 캐스팅 (is, as, as?, as!)

## 1. 타입 캐스팅 연산자

**타입 캐스팅(Type Casting)**은 인스턴스의 타입을 확인하거나, 해당 인스턴스를 자신의 클래스 계층에 있는 **상위** 혹은 **하위** 클래스로 처리하는 방법이다.

스위프트의 타입 캐스팅은 is 와 as 연산자로 구현된다. 이 두 연산자는 값의 타입을 확인하거나 값을 다른 타입으로 지정한다.

또한 타입 캐스팅을 사용하여 해당 타입이 프로토콜을 따르는지 (Protocol Conformance) 여부도 확인할 수 있다.

타입 캐스팅 연산자는 총 4가지가 있다.

```swift
expression is type
expression as type
expression as? type
expression as! type
```

- **is** 연산자 : 런타임에 expression 이 특정 type 으로 캐스팅 되는지 체크한다. 가능하면 true 불가능하면 _false_를 return 한다.

- **as** 연산자 : 컴파일 단계에서 캐스팅이 실행된다. 따라서 언제나 특정 type 으로 캐스팅이 성공할 때만 사용이 가능하다. 업캐스팅(Upcasting) 혹은 브릿징(Bridging) 에 사용된다.

- **as?** 연산자 : 런타임에 캐스팅하여 특정 type 의 옵셔널을 반환한다. 성공하면 옵셔널 값을 반환하고 실패하면 nil 을 반환한다.

- **as!** 연산자 : 런타임에 특정 type 으로 강제 캐스팅한다. 캐스팅에 실패할 경우 런타임 에러가 발생할 수 있다.

`업캐스팅(Upcasting)` : 해당 타입의 상위 타입의 인스턴스로 캐스팅

`브릿징(Bridging)` : NSString과 같은 Foundation 타입을 String과 같은 스위프트 표준 라이브러리로 캐스팅

## 2. 타입 체크와 다운캐스팅 (Checking Type & Downcasting)

타입 캐스팅을 위해 클래스 계층 예시를 만들어 보자.

먼저 상위 클래스인 MediaItem을 정의한다.

```swift
class MediaItem { 
    var name: String

    init(name: String) { 
        self.name = name 
    } 
} 
```

다음 MediaItem 클래스를 상속받는 Movie 와 Song 클래스 두 개를 선언한다.

```swift
class Movie: MediaItem {
    var director: String

    init(name: String, director: String) {
        self.director = director
        super.init(name: name)
    }
}

class Song: MediaItem {
    var artist: String

    init(name: String, artist: String) {
        self.artist = artist
        super.init(name: name)
    }
}
```

생성자는 MediaItem 클래스를 상속받기 때문에 공통적으로 name 을 받아 초기화하며 Movie 클래스의 경우 추가적으로 director 를 받아 초기화하고 Song 클래스의 경우 artist 를 받아 초기화한다.
 
마지막으로, Movie 와 Song 인스턴스를 담는 library 배열을 생성한다.

```swift
let library = [ 
    Movie(name: "Casablanca", director: "Michael Curtiz"), 
    Song(name: "Blue Suede Shoes", artist: "Elvis Presley"), 
    Movie(name: "Citizen Kane", director: "Orson Welles"), 
    Song(name: "The One And Only", artist: "Chesney Hawkes"), 
    Song(name: "Never Gonna Give You Up", artist: "Rick Astley") 
] 
// the type of "library" is inferred to be [MediaItem]
```

스위프트의 타입 체커는 Movie 와 Song 이 공통적으로 MediaItem 을 부모클래스로 가지는 것을 알고 library 의 타입을 [MediaItem] 으로 추론한다.

그렇기 때문에 실제로 해당 배열에서 값을 꺼내서 쓸 때는 MediaItem으로 추출이 되기 때문에, Movie나 Song으로 사용하려면 다운캐스팅을 해주어야 한다.

## 3. is 연산자로 타입 체크하기

타입 체크를 할 때는 is 연산자를 활용하면 인스턴스가 특정 하위 클래스 타입인지 확인한다. 인스턴스가 하위 클래스 유형이면 true 를 반환하고 아니면 false 를 반환한다.

```swift
var movieCount = 0 
var songCount = 0 

for item in library { 
    if item is Movie { 
        movieCount += 1 
    } else if item is Song { 
        songCount += 1 
    } 
} 

print("Media library contains \(movieCount) movies and \(songCount) songs") 
// Prints "Media library contains 2 movies and 3 songs"
```

예시를 통해 확인해보면 library 배열에는 Movie 인스턴스가 2개, Song 인스턴스가 3개가 들어있는 것을 확인할 수 있다.

## 4. as 연산자로 다운캐스팅 하기

특정 클래스 타입의 상수나 변수가 실제로는 해당 인스턴스의 하위클래스를 나타내고 있을 수 있다. 이 때 as? 나 as! 연산자를 사용하여 하위 클래스로 다운캐스팅하여 하위클래스로 해당 인스턴스를 사용할 수 있다.
 
다운캐스팅은 실패할 수도 있기때문에, 두 가지 연산자가 존재한다. as? 의 경우 옵셔널 값을 반환하고 as! 의 경우 강제 추출(force-unwrap)한 값을 반환하게 된다. as! 사용시 실패했을 때 옵셔널에 대한 개념이 있다면 런타임 에러의 위험이 있다는 것을 알 수 있다.
 
따라서 다운캐스팅 성공에 대한 확신이 있을 때는 as! 를 사용하고 그렇지 않을 경우엔 as? 를 사용하면 된다. 절대적인 확신이 없다면 옵셔널 값으로 반환시키는게 비교적 안전하다.
 
예시를 통해 다운캐스팅을 어떻게 이용하는지 살펴보자.

```swift
for item in library { 
    if let movie = item as? Movie { 
        print("Movie: \(movie.name), dir. \(movie.director)") 
    } else if let song = item as? Song { 
        print("Song: \(song.name), by \(song.artist)") 
    } 
} 

// Movie: Casablanca, dir. Michael Curtiz 
// Song: Blue Suede Shoes, by Elvis Presley 
// Movie: Citizen Kane, dir. Orson Welles 
// Song: The One And Only, by Chesney Hawkes 
// Song: Never Gonna Give You Up, by Rick Astley
```
as? 연산자를 이용하여 다운캐스팅 하고 반환된 옵셔널 값을 if-let 구문으로 처리하는 방법이다. 다운캐스팅 하는 가장 일반적인 방법으로 볼 수 있다.
 
캐스팅은 실제로 인스턴스를 수정하거나 그 값을 변경하지 않는다. 단순히 캐스팅 된 타입으로 해당 인스턴스를 처리하고 접근할 뿐이다.

## 5. Any나 AnyObject를 타입 캐스팅 하는 방법

스위프트는 불특정한 타입을 다루기 위해 두 가지 특별한 타입을 제공한다.

- Any 는 모든 타입의 인스턴스를 나타낼 수 있다. (함수 타입 포함)
- AnyObject 는 모든 클래스 타입의 인스턴스를 나타낼 수 있다.

Any나 AnyObject로 선언하기 보다는 예상되는 타입을 구체적으로 지정하는 것이 더 좋다.
 
Any 타입의 값들을 저장하는 배열 예제를 살펴보자.

```swift
var things = [Any]() 

things.append(0) 
things.append(0.0) 
things.append(42) 
things.append(3.14159) 
things.append("hello") 
things.append((3.0, 5.0)) 
things.append(Movie(name: "Ghostbusters", director: "Ivan Reitman")) 
things.append({ (name: String) -> String in "Hello, \(name)" }) 
```

things 배열에서 특정 타입의 값을 찾아내기 위해서는 is나 as를 이용한 switch구문을 이용하면 된다.

```swift
for thing in things { 
    switch thing { 
    case 0 as Int: 
        print("zero as an Int") 
    case 0 as Double: 
        print("zero as a Double") 
    case let someInt as Int: 
        print("an integer value of \(someInt)") 
    case let someDouble as Double where someDouble > 0: 
        print("a positive double value of \(someDouble)") 
    case is Double: 
        print("some other double value that I don't want to print") 
    case let someString as String: 
        print("a string value of \"\(someString)\"") 
    case let (x, y) as (Double, Double): 
        print("an (x, y) point at \(x), \(y)") 
    case let movie as Movie: 
        print("a movie called \(movie.name), dir. \(movie.director)") 
    case let stringConverter as (String) -> String: 
        print(stringConverter("Michael")) 
    default: 
        print("something else") 
    } 
}

// zero as an Int 
// zero as a Double 
// an integer value of 42 
// a positive double value of 3.14159 
// a string value of "hello" 
// an (x, y) point at 3.0, 5.0 
// a movie called Ghostbusters, dir. Ivan Reitman 
// Hello, Michael 
```

Any 타입은 옵셔널 타입을 포함한 모든 타입을 나타낼 수 있다. 값 타입이 Any 타입으로 기대되는 곳에서 만약 옵셔널 값을 사용한다면 스위프트는 warning 을 나타내는데, 이 때는 아래 처럼 as 연산자를 사용하여 옵셔널 타입을 Any 로 명시적 캐스팅하여 사용하면 된다. 

```swift
let optionalNumber: Int? = 3 
things.append(optionalNumber)        // Warning 
things.append(optionalNumber as Any) // No warning
```


---

### 참고자료

[[Swift] Type Casting (is, as, as?, as!) 타입캐스팅 완벽 정리](https://onelife2live.tistory.com/9)
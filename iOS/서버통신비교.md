#  기본 과제 : 서버통신 URLSession, Alamofire, Moya 비교하기

iOS에서는 서버와 통신하기 위해 기본적으로 Foundation의 `URLSession`이라는 API를 사용하고 있다. `URLSession`은 로우레벨의 코드를 작성할 수 있고, 다른 프레임워크를 사용할 필요가 없다는 장점이 있지만 사용이 복잡하고 코드의 가독성이 좋지 않다는 단점이 있다.

따라서 `URLSession`을 기반으로 한 단계 추상화시킨 방식으로 한네트워킹 작업을 단순화해주는 라이브러리인 `Alamofire` 라이브러리를 보편적으로 사용한다. 하지만 `Alamofire`는 유지 보수와 유닛 테스트가 힘들다는 단점을 가지고 있다.

이때 `URLSession`을 추상화한 `Alamofire`를 다시 추상화한 프레임워크로 NetWork Layer를 템플릿화 해서 재사용성을 높히고, 테스트가 용이하며 개발자가 request,response에만 신경쓰도록 해준 것이 바로 `Moya`라이브러리이다.

iOS 서버통신에 자주 사용되는 `URLSession`, `Alamofire`, `Moya`에 대해 간단히 비교해보도록 하자.

> **참고💡** 추상화란?
> 
> : 객체들의 공통된 부분만 따로 뽑아, 재사용을 하기 쉽도록 구현하는 것을 뜻한다. (쉽게 말해 일반화시키는 것)
> 
> **추상화의 장점** : 모델링, 코드의 재사용성, 코드의 가독성, 일관된 방향성 등이 공통적으로 언급이 된다.
> 
> 모델링을 통해 코드를 추상화하면 코드의 재사용성을 높일 수 있고, 코드의 가독성을 높여 코드를 이해하기 훨씬 더 쉽게 만들어 주기도 한다. 그리고 추상화된 코드는 자연스럽게 일관성을 가지게 된다.

### 잠깐! HTTP, REST, 그리고 JSON에 대해 간락히 알고 넘어가자

**HTTP**는 서버에서 클라이언트로 데이터를 전송할 때 사용하는 Application Protocol이다. HTTP는 아래와 같이 다양한 request method를 정의하여 바람직한 동작들을 가리킬 수 있게 한다.

- **GET**: 데이터를 받는다. (서버의 데이터를 변경할 수는 없다.)
- **HEAD**: GET과 비슷하지만, 진짜 데이터가 아닌 header만 전달한다.
- **POST**: 데이터를 서버에 전송한다. (ex. form을 채우거나 submit 버튼을 누를 때 등)
- **PUT**: 데이터를 특정한 장소에 전송한다. (ex. user profile 업데이트 등)
- **DELETE**: 특정 장소의 데이터를 삭제한다.

**JSON**은 JavaScript Object Notation의 약자로, 시스템 간 데이터 전달에 있어 직관적이고 사람이 읽을 수 있는 메커니즘을 제공한다. JSON은 string, boolean, array, object/dictionary, number, null과 같이 한정된 수의 데이터 타입만 가질 수 있다.

Swift4 전에는 JSON에서 data object로, 또 그 반대로 변환하기 위해 JSONSerialization 클래스를 사용해야 했는데, 요즘은 Codable 프로토콜을 채택해 JSON과 data model 사이 자동화 변환을 이용한다.

**REST**는 REpresentational State Transfer의 약자로, 지속적인 웹 API를 만들기 위한 규칙의 집합이다. REST는 request 사이에 상태를 지속하지 않거나, cacheable request를 만들고, 동일한 인터페이스를 제공한다. 이를 통해 request간 데이터의 상태를 추적하지 않고도 API를 우리가 만든 앱에 통합하는 것을 쉽게할 수 있도록 한다.

---

## 1. URLSession

- URLSession은 HTTP/HTTPS를 통해 콘텐츠 및 데이터를 주고받기 위해 API를 제공하는 클래스 및 클래스 모음이다.

```swift
class URLSession : NSObject
```

> An object that coordinates a group of related, network data transfer tasks.

<img src="https://koenig-media.raywenderlich.com/uploads/2019/05/02-URLSession-Diagram.png">

`URLSession` : HTTP 요청을 보내고 받는 핵심 개체이다. 제공되는 `URLSessionConfiguration`을 통해 다음 세 가지 유형의 URL을 생성한다.

- `.default`: 기본 네트워크 통신
- `.ephemeral`: 쿠키나 캐시를 저장하지 않게 할 때 사용 (private 모드와 비슷하다)
- `.background`: 앱이 백그라운드에 있을 때 사용 (컨텐츠 다운로드 혹은 업로드 등)

`URLSession` 여러개로 `URLSessionTask`를 만들 수 있다. 이 `URLSessionTask`로 실제 통신을 하게 된다. `URLSessionTask`도 세 가지 유형으로 분류할 수 있다.

- `URLSessionDataTask` : 간단한 데이터를 받아올 때 사용 (백그라운드에서 진행은 안 됨)
- `URLSessionUploadTask` : 데이터를 업로드할 때 사용
- `URLSessionDownloadTask` : 데이터를 다운도르 할 때 사용

`URLSession Delegate`을 통해서 네트워크 중간과정을 확인할 수 있다. (필수는 아님)

> URLSession 실습

```swift
let config = URLSessionConfiguration.default
let session = URLSession(configuration: config)

var urlComponents = URLComponents(string: "https://itunes.apple.com/search?media=music&entity=song&term=IU")!
let requestURL = urlComponents.url!
```

위의 코드를 통해 URLConfiguration의 객체를 생성하고 이를 통해 URLSession을 생성한 것을 확인할 수 있다.

위의 requestURL에는 아래와 같이 50건의 IU님의 곡에 대한 정보가 나와있다.

<img src="https://velog.velcdn.com/images%2Ffolw159%2Fpost%2Fc09ab833-ac1b-4dd4-9f08-4200e521b240%2Fimage.png">

여기서 원하는 정보만을 뽑아내기 위해 **Codable** 프로토콜을 채택한 구조체를 생성한다. 

```swift
struct Response: Codable {
    let resultCount: Int
    let tracks: [Track]
    
    enum CodingKeys: String, CodingKey {
        case resultCount
        case tracks = "results"
    }
}

struct Track: Codable {
    let title: String
    let artistName: String
    
    enum CodingKeys: String, CodingKey {
        case title = "trackName"
        case artistName
    }
```

그리고 DataTask를 생성하여 데이터를 가져온다.

```swift
// data task 생성
let dataTask = session.dataTask(with: requestURL) { (data, response, error) in

    guard error == nil else {
        return
    }
    
    // HTTP 응답 여부 확인
    guard let statusCode = (response as? HTTPURLResponse)?.statusCode else {
        return
    }
    
    // HTTP 응답 성공 범위
    let successRange = 200..<300
    
    guard successRange.contains(statusCode) else {
        return
    }
    
    // 네트워크를 통해 받은 데이터를 resultData에 저장
    guard let resultData = data else { return }
    
    // 데이터 파싱 및 결과 출력
    do {
        let decoder = JSONDecoder()
        let response = try decoder.decode(Response.self, from: resultData)
        let tracks = response.tracks
        
        print("--> tracks: \(tracks)")
    } catch let error {
        print("---> error: \(error.localizedDescription)")
    }
}

dataTask.resume()
```

위의 코드를 실행하면 아래와 같이 곡 제목, 가수만 50건이 출력된다.

```
--> tracks: [__lldb_expr_31.Track(title: "Love Poem", artistName: "IU"),
...
]
```

<br>

---

## 2. Alamofire

Alamore란 비동기로 수행하는 Swift 기반의 HTTP 네트워킹 라이브러리이다.

Alamofire는 URLSession 기반이며,URLSession 및 URLSessionTask 같은 클래스를 사용하기 쉽게 구현되어 있다. 

- Alamofire는 CocoaPods를 사용해 쉽게 설치 가능하다.

```swift
pod 'Alamofire', '~> 5.2' // 5.2 version
```

cf. 설치한 라이브러리를 추가할 때 : `import Alamofire`

제공되는 대표 기능으로는 아래와 같다.

- `AF.upload` : 멀티파트, 스트림, 파일메소드를 통해 파일을 업로드한다.
- `AF.download` : 파일을 다운로드하거나 이미 진행 중인 다운로드를 재개한다.
- `AF.request` : 파일 전송과 무관한 다른 HTTP를 요청한다.

> 인스타그램 클론 과제 회원가입 부분 예시

```swift
        let dataRequest = AF.request(url,
                                     method: .post,
                                     parameters: body,
                                     encoding: JSONEncoding.default,
                                     headers: header)

        dataRequest.responseData { dataResponse in
            switch dataResponse.result {
            case .success:
                guard let statusCode = dataResponse.response?.statusCode else { return }
                guard let value = dataResponse.value else { return }

                let networkResult = NetworkHelper.parseJSON(by: statusCode, data: value, type: SignUpResponse.self)
                completion(networkResult)

            case .failure(let err):
                print(err)
                completion(.networkFail)
            }
        }
```

<br>

---

## 3. Moya

Moya는 URLSession을 추상화한 Alamofire를, 다시 추상화한 라이브러리로 Network Layer를 템플릿으로 만들어 재사용성을 높이고 개발자가 request, response에만 집중할 수 있도록 설계한 라이브러리이다.

Moya 공식문서에 있는 대로 순서를 정리해보도록 하자.

### 1. Service.swift 파일을 만든다.

각 case는 개별적인 네트워크를 담당하게 된다. 파라미터는 개별 API 문서를 보고 필요한 경우에 따라 혹은 로직에 따라서 만든다.

```swift
enum CardService {
    case cardDetailFetch(cardID: String)
    case cardCreation(request: CardCreationRequest, image: UIImage)
    case cardListEdit(request: CardListEditRequest)
    case cardDelete(cardID: String)
}
```

### 2. extension을 통해 TargetType프로토콜을 추가로 준수하도록 하고, 필요한 속성을 Service.swift 에 추가로 구현한다.

TargetType 프로토콜을 채택하는 이유는 아래와 같이 다양한 네트워킹 속성을 제공하기 때문인데, 아래와 같은 네트워킹 속성을 가진다.

- baseURL : 서버의 base URL
- path : 서버의 base URL 뒤에 추가될 Path
- method : HTTP Method (GET, POST, PUT, DELETE 등...)
- task : request에 사용되는 파라미터 설정
- sampleData : 테스트용 Mock Data (테스트를 위한 목업 데이터를 제공할 때 사용)
- validationType : 허용할 response의 타입
- headers : HTTP headers


```swift
extension CardService: TargetType {
    
    var baseURL: URL { return URL(string: Const.URL.baseURL)! }
    
    var path: String {
        switch self {
        case .cardDetailFetch(let cardID):
            return "/card/\\(cardID)"
        case .cardCreation:
            return "/card"
        case .cardListEdit:
            return "/cards"
        case .cardDelete(let cardID):
            return "/card/\\(cardID)"
        }
    }
    
    var method: Moya.Method {
        switch self {
        case .cardDetailFetch:
            return .get
        case .cardCreation:
            return .post
        case .cardListEdit:
            return .put
        case .cardDelete:
            return .delete
        }
    }
    
    var sampleData: Data {
        return Data()
    }
    
    var task: Task {
        switch self {
        case .cardDetailFetch, .cardDelete:
            return .requestPlain
        case .cardCreation(let request, let image):
            
            var multiPartData: [Moya.MultipartFormData] = []
            
            let userIDData = request.userID.data(using: .utf8) ?? Data()
            multiPartData.append(MultipartFormData(provider: .data(userIDData), name: "card.userId"))
            let defaultImageData = Int(request.frontCard.defaultImage).description.data(using: .utf8) ?? Data()
            multiPartData.append(MultipartFormData(provider: .data(defaultImageData), name: "card.defaultImage"))
             "card.thirdTMI"))
        
            return .uploadMultipart(multiPartData)
        case .cardListFetch(let userID, let isList, let offset):
            return .requestParameters(parameters: ["userId": userID,
                                                   "list": isList ?? false,
                                                   "offset": offset ?? ""
            ], encoding: URLEncoding.queryString)
        case .cardListEdit(let requestModel):
            return .requestJSONEncodable(requestModel)
        }
    }
    
    var headers: [String: String]? {
        switch self {
        case .cardDetailFetch, .cardDelete:
            return .none
        case .cardCreation:
            return ["Content-Type": "multipart/form-data"]
        case .cardListEdit:
            return ["Content-Type": "application/json"]
        }
    }
}
```

### 3. 제네릭 타입으로 Service를 가진 MoyaProvider 인스턴스를 생성한다.


```swift
var cardProvider = MoyaProvider<CardService>(plugins: [NetworkLoggerPlugin()])

provider.request(.createUser(firstName: "James", lastName: "Potter")) { result in
    // do something with the result (read on for more details)
}
```

> **참고💡** NetworkLoggerPlugin란?
> 
> : 발생하는 모든 네트워크 작업을 콘솔에 기록해주는 것을 뜻한다.

### 4. 서버 통신을 진행하는 파일을 만들고, 서버 통신을 한다.


```swift
import Foundation
import Moya

public class CardAPI {
    static let shared = CardAPI()
    var cardProvider = MoyaProvider<CardService>(plugins: [MoyaLoggerPlugin()])
    
    public init() { }
    
    func cardDetailFetch(cardID: String, completion: @escaping (NetworkResult<Any>) -> Void) {
        cardProvider.request(.cardDetailFetch(cardID: cardID)) { (result) in
            switch result {
            case .success(let response):
                let statusCode = response.statusCode
                let data = response.data
                
                let networkResult = self.judgeCardDetailFetchStatus(by: statusCode, data)
                completion(networkResult)
                
            case .failure(let err):
                print(err)
            }
        }
    }
    
    func cardCreation(request: CardCreationRequest, image: UIImage, completion: @escaping (NetworkResult<Any>) -> Void) {
        cardProvider.request(.cardCreation(request: request, image: image)) { (result) in
            switch result {
            case .success(let response):
                let statusCode = response.statusCode
                let data = response.data

                let networkResult = self.judgeCardCreationStatus(by: statusCode, data)
                completion(networkResult)
                
            case .failure(let err):
                print(err)
                completion(.networkFail)
            }
        }
    }
    
    func cardListEdit(request: CardListEditRequest, completion: @escaping (NetworkResult<Any>) -> Void) {
        cardProvider.request(.cardListEdit(request: request)) { (result) in
            switch result {
            case .success(let response):
                let statusCode = response.statusCode
                let data = response.data
                
                let networkResult = self.judgeStatus(by: statusCode, data)
                completion(networkResult)
                
            case .failure(let err):
                print(err)
            }
        }
    }
    
    func cardDelete(cardID: String, completion: @escaping (NetworkResult<Any>) -> Void) {
        cardProvider.request(.cardDelete(cardID: cardID)) { (result) in
            switch result {
            case .success(let response):
                let statusCode = response.statusCode
                let data = response.data
                
                let networkResult = self.judgeStatus(by: statusCode, data)
                completion(networkResult)
                
            case .failure(let err):
                print(err)
            }
        }
    }
    
    private func judgeCardDetailFetchStatus(by statusCode: Int, _ data: Data) -> NetworkResult<Any> {
        
        let decoder = JSONDecoder()
        guard let decodedData = try? decoder.decode(GenericResponse<Card>.self, from: data)
        else {
            return .pathErr
        }
        
        switch statusCode {
        case 200:
            return .success(decodedData.data ?? "None-Data")
        case 400..<500:
            return .requestErr(decodedData.msg)
        case 500:
            return .serverErr
        default:
            return .networkFail
        }
    }
    
    private func judgeCardCreationStatus(by statusCode: Int, _ data: Data) -> NetworkResult<Any> {
        
        let decoder = JSONDecoder()
        guard let decodedData = try? decoder.decode(GenericResponse<Card>.self, from: data)
        else {
            return .pathErr
        }
        
        switch statusCode {
        case 201:
            return .success(decodedData.data ?? "None-Data")
        case 400..<500:
            return .requestErr(decodedData.msg)
        case 500:
            return .serverErr
        default:
            return .networkFail
        }
    }
    
    private func judgeStatus(by statusCode: Int, _ data: Data) -> NetworkResult<Any> {
        let decoder = JSONDecoder()
        guard let decodedData = try? decoder.decode(GenericResponse<String>.self, from: data)
        else { return .pathErr }
        
        switch statusCode {
        case 200:
            return .success(decodedData.msg)
        case 400..<500:
            return .requestErr(decodedData.msg)
        case 500:
            return .serverErr
        default:
            return .networkFail
        }
    }
}
```


---

### 참고자료

[URLSession | Apple Developer Documentation](https://developer.apple.com/documentation/foundation/urlsession)

[Alamofire 깃허브 공식문서](https://github.com/Alamofire/Alamofire)

[Moya 깃허브 공식문서](https://github.com/Moya/Moya)

[[iOS - swift] 1. Alamofire 사용 방법 - Network Layer 구현 (Moya 프레임워크처럼 사용하는 방법)](https://ios-development.tistory.com/731)

[[Swift] Alamofire를 Moya처럼 사용해보자! By Router Pattern (1편 - Foundation Setting)](https://jazz-the-it.tistory.com/25)

[[Swift] Alamofire를 Moya처럼 사용해보자! By Router Pattern (2편 - Services, Routers 구현)](https://jazz-the-it.tistory.com/26?category=1003495)

[Alamofire 5 Tutorial for iOS: Getting Started](https://www.raywenderlich.com/6587213-alamofire-5-tutorial-for-ios-getting-started#toc-anchor-009)

[Fetching Website Data into Memory](https://developer.apple.com/documentation/foundation/url_loading_system/fetching_website_data_into_memory)

[Swift, URLSession가 무엇인지, 어떻게 사용하는지 알아봅니다.](https://devmjun.github.io/archive/URLsession)

[iOS URLSession 이해하기](https://hcn1519.github.io/articles/2017-07/iOS_URLSession)

[[iOS - swift] URLSession 네트워크 통신 기본 (URLSessionConfiguration, URLSession, URLComponents, URLSessionTask)](https://ios-development.tistory.com/651)

[[iOS] Moya가 모야? - Moya로 Get 통신하기](https://roniruny.tistory.com/150)

[[iOS] Moya , Alamofire , URLSession 비교](https://velog.io/@heyksw/iOS-Moya-Alamofire-URLSession-비교)

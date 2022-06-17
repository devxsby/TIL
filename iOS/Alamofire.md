# Alamofire 라이브러리

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

## 1. 

`업데이트 예정`


---

### 참고자료

Alamofire 공식 깃허브 : https://github.com/Alamofire/Alamofire

[[Swift] Alamofire 알아보기](https://velog.io/@dlwns33/Swift-Alamofire-알아보기)

[iOS Swift 라이브러리 Alamofire 사용하기](https://gonslab.tistory.com/14)

[Swift Alamofire 시작하기](https://yoonjong.tistory.com/entry/Swift-Alamofire-시작하기)
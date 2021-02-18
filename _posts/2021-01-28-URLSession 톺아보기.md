---
title: "URLSession 톺아보고 통신해보기!"
date: 2021-01-28 21:14:28 -0400
categories: URLSession 
toc: true
tags:
 -URLSession이란?
 -URLSessionTask란?
 -URLSession으로 통신해보기
---

 Alamofire는 모두가 유용하게 사용하고 있는 네트워킹 라이브러리입니다. 근데 이 라이브러리를 뜯어보면 URLSession으로 시작된 것을 아시나요? 아주 신기합니당 그래서 오늘은 URLSession에 대해서 알아보고 Request랑 Response받는것도 알아보려고 합니다!



![image](https://user-images.githubusercontent.com/49550838/106137152-ac20cf80-61ad-11eb-955f-c03e995ff695.png)



 전체적으로 살펴볼 내용은 이 사진에서 보여지는 URLSession과 URLSessionTask입니다.



### URLSession이란?

 HTTP/HTTPS를 통해 콘텐츠를 주고받는 API를 제공하는 클래스입니다. 앱은 이 API를 사용하여 앱이 실행되고 있지 않거나 iOS에서 앱이 일시 중지된 동안 백그라운드 다운로드를 수행할 수 있습니다. 관련 및 인증을 지원하고 리디렉션 및 작업완료와 같은 이벤트를 수신할 수 있습니다.



#### URLSession 유형(URLSessionConfiguration을 통해)

- 기본 세션 - Default Session

  공유 세션과 매우 유사하게 작동하지만 데이터를 점진적으로 가져오기위해 기본 세션에 delegate를 지정할 수도 있습니다. 

- 임시 세션 - Ephemeral Session

  공유 세션과 유사하지만 캐시, 쿠키 또는 자격 증명을 디스크에 쓰지 않습니다.

- 백그라운드 세션 - Background Session

  앱이 실행되지 않는 동안 백드라운드에서 콘텐츠 업로드 및 다운로드를 수행할 수 있습니다.



#### URLSession 작업 유형

- 데이터 작업

  NSData 개체를 사용하여 데이터를 보내고 받습니다.

- 업로드 작업

  데이터를 전송하고 앱이 실행되지 않는 동안 백그라운드 업로드를 지원합니다

- 다운로드 작업

  파일 형식으로 데이터를 검색하고 앱이 실행되지 않는동안 백그라운드 다운로드 및 업로드를 지원합니다.



### URLSessionTask란?

URLSession에서 수행되는 특정 리소스 다운로드와 같은 작업으로 세션은 데이터를 가져오거나 파일을 업로드, 다운로드하는 실제 작업을 수행하는 하나 이상의 Task를 생성합니다.



#### URLSessionTask 유형

- URLSessionDataTask 

  서버에서 메모리로 데이터를 검색하는 HTTP GET 요청에 사용

- URLSessionUploadTask

  전형적으로 HTTP POST, PUT 메소드를 통해서 디시크에서 웹서버로 파일을 전송할 때 사용

- URLSessionDownloadTask

  임시의 파일 위치로 원격 서버에서 파일을 다운로드할 때 사용



 Apple 공식문서를 보고 내용들을 정리해보았는데 이제는 URLSession을 이용하여 Request, Response를 구현해보도록 하겠습니다.



### URLSession으로 통신해보기

URLSession은 Request 할 때 URL 객체를 통해 직접 통신하는 형태와 URLRequest 객체를 만들어서 옵션을 설정하는 형태가 있습니다. 

다음으로 Response는 설정된 Task의 Completion Handler 형태로 response를 받는 형태, URLSessionDelegate를 통해 지정된 메소드를 호출하는 형태가 있습니다. 



이번엔 URLRequest 객체를 만들어서 옵션을 설정하는 형태와 Completion Handler 형태로 request와 response를 받아보겠습니다.

```swift
let session: URLSession = URLSession(configuration: .default)
var task: URLSessionDataTask?
var components = URLComponents(string: "http://urlsession.example.com")
//var query = URLQueryITem(name: "", value: "") 만약 쿼리가 필요하다면
let url = URLRequest(url: components?.url)

task = session.dataTask(with: url) { [weak self] data, response, error in
     guard let data = data, let dic = try?JSONSerialization.jsonObject(with: data) as? [String:Any]
                             
     print(data)
     //원하는 작업으로 data 뽑기                               
}

task?.resume()
```

1. Session 생성, task 생성. 그리고  환경설정을 해줍니다
2. URLComponents로 URL을 구성합니다. 쿼리가 있다면 쿼리를 설정해줍니다. URLComponent에 관련한 설명은 [여기](https://zeddios.tistory.com/1103)가 더 좋을 것 같아 첨부합니다.
3. 이번엔 URLRequest 객체를 만들어서 옵션을 설정한다고 했으니 URLRequest 기반으로 객체를 만들어줍니다.
4. dataTask(with: ) 를 통해 데이터를 주고 받습니다. JSONSerialization은 json 데이터를 String 형태로 변환하여 Swift에서 사용할 수 있도록 하는 것으로 data를 변환해줍니다.
5. 변환한 데이터에 대한 작업을 해서 핸들러를 마무리하고 resume()으로 만들어 dataTask가 시작할 수 있도록 합니다. 모든 작업은 기본적으로 일시정지 상태로 시작하기 때문입니다.



#### 출처 

https://devmjun.github.io/archive/URLsession

https://hcn1519.github.io/articles/2017-07/iOS_URLSession

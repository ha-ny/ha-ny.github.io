---
layout: post
title: App version check
author: Hany
tags:
  - Swift
---
[Tutoring Board](https://ha-ny.github.io/2023-10-25/Tutoring-Board-%EA%B3%BC%EC%99%B8-%EC%9D%BC%EC%A0%95-%EA%B4%80%EB%A6%AC)  : 1.0.0에서 1.0.1로 업데이트 후 친구 폰으로 테스트하려는데 업데이트 한 줄 몰랐단당.. <br>
그래서 다음 업데이트에는 꼭 최신 버전 체크해서 사용자에게 업데이트 알림을 띄워줘야지! 생각했다
<br>
<br>


# 설명

다양한 방법이 있지만 major, minor까지만 체크하는 방법이 유독 많은 것 같다! patch의 경우 비교적 자주 업데이트되기 때문에 앱 실행할 때마다 업데이트 유도 알림이 뜨면 사용자의 피로도가 올라갈 것이다.<br>

앱 업데이트는 강제 업데이트 / 선택 업데이트로 크게 나눌 수 있는데 택 1의 개념보다는 major는 강제 업데이트, minor는 선택 업데이트처럼 섞어서 사용하는 것 같다. 강제 업데이트는 런타임 오류가 발생하는 오류나 앱 내부의 중요한 요소가 바뀔 때 등 꼭 업데이트가 필요할 경우에 사용한다. 선택 업데이트는 다음에 또는 1일 후 다시 알림과 지금 업데이트처럼 업데이트를 미루고 앱을 사용할 수 있는 선택지를 준다.<br>

개인적인 이야기지만 나는 앱을 실행했을 때 강제 업데이트 Alert이 뜨면 의욕이 뚝 떨어진다. 하지만 선택 업데이트일 땐 계속.. 계속 미루는 편이라 매번 뜨는 알림이 귀찮아서 앱을 지우는 경향이 있다. 이렇듯 사용자 케이스는 정말 다양하기 떄문에 가장 보편적인 케이스로 대응하는 방법이 강제/선택 업데이트를 적절히 적용하는 것이다.<br>

Version 체크는 대체로 점(.)을 기준으로 나눈 후 사용자 앱의 major 버전과 최신 major 버전을 비교하는 방식으로 진행된다. patch의 최신 버전이 4라고 했을 때 현재 앱 버전이 3이라면 업데이트가 필요한 상태 일 것이다.<br>
하지만 나는 버전을 한 번에 비교할 수 있는 방법이 궁금했고, 그 결과  compare을 사용하는 방법을 선택했다.<br>

예전에 작성해 둔 compare과 ComparisonResult 설명이다.<br>
->  [compare](https://ha-ny.github.io/2023-10-01/compare)

- compare을 사용하는 방법과 점을 기준으로 버전을 비교하는 방식은 뭐가 다를까?
가볍게 생각한다면 major를 비교하는 기능을 만들어둔 상태라면 minor 또는 patch까지 범위를 늘리기는 쉬울 것이다! 반대로 major 대신 patch만 비교하는 코드로 변경하는 것도 쉬울 텐데 compare을 사용한 상태에서 major만 비교해야 한다면?.. 유지 보수의 관점에서 compare로 버전을 비교하는 것은 추천하지 않을 것 같다.<br>

강제 업데이트는 앱 진입을 막고, 앱스토어를 연결해서 앱을 사용하지 못하게 하기 때문에 진입을 막으려면 AppDelegate 또는 LaunchScreen에서진행하면 된다. 하지만 나는 캘린더 뷰를 백그라운드처럼 깔고 Alert을 띄우고 싶었다.<br>

그래서 아래 View 코드는 CalendarViewController의 viewDidLoad에서 실행한다.<br>

개인 앱의 장점은 내 입맛에 맞춰도 되는 것 아닐까?? 강제 업데이트 +  compare 비교 코드로 작성했다<br>
1. 선택 업데이트를 진행하지 않은 이유:  앱이 온전히 자리 잡기 전이라 버그 잡느라 바쁘다 바빠. 가끔 사용자의 데이터와 직결되는 문제가 있어, 당분간은 강제적으로 업데이트를 진행하려고 한다.
2. compare 비교 코드를 작성한 이유: 위에서 말한 것처럼 버전을 한 번에 비교할 수 있는 방법이 궁금했고, 예전에 공부해둔 compare을 제대로 사용해 볼 좋은 기회였다! 다른 개발자분들의 Version 체크 코드를 많이 둘러봤으니 요정도는 내 재량이라고 생각한다.
<br>

# 코드
## View

```swift
private func appVersionCheck() {
    AppVersionCheck.updateRequired { url in
            DispatchQueue.main.async { [weak self] in
                guard let self else { return }
                if let url {
                    UIAlertController.customMessageAlert(view: self, title: "appVersionCheckTitle".localized, message: "appVersionCheckMessage".localized) {
                        if UIApplication.shared.canOpenURL(url) {
                            UIApplication.shared.open(url, options: [:]) { _ in
                                exit(0)
                        }
                    }
                }
            }
        }
    }
}
```

UIApplication.shared.open의 CompeletionHandler를 사용해서 앱스토어로 화면이 넘어간 후 exit 코드가 작동한다. 화면이 넘어간 후 exit 코드를 읽기 때문에 코드를 읽었음에도 앱에 다시 돌아왔을 때 강제 종료가 아닌 팝업이 그대로 남아있게 된다. 아무리 찾아봐도 왜 이렇게 되는지 찾을 수 없었지만 일단 내가 원하는 대로 작동하기 때문에 냅둔다 ..ㅎㅎ (exit은 애플이 권장하지 않는 방법 중 하나이기 때문에 아마 정상적인 코드 실행이 아닐 땐 작동하지 않는 것 같기도 하다.)
<br>

## AppVersionCheck

```swift
class AppVersionCheck {

    static private let appleID = "123456789"

    //업데이트 필요 여부 확인
    static func updateRequired(_ completion: @escaping (URL?) -> ()) {
        guard let oldVersion = Bundle.main.infoDictionary?["CFBundleShortVersionString"] as? String else { return completion(nil) }

        getLatestAppStoreVersion { latestVersion in
            guard let latestVersion else { return completion(nil) }
            
            let compareResult = oldVersion.compare(latestVersion, options: .numeric)
            switch compareResult {
            case .orderedAscending: //앱 스토어 보다 낮은 버전(업데이트 필요)
                guard let url = URL(string: "itms-apps://itunes.apple.com/app/apple-store/\(appleID)") else { return completion(nil)}
                
                return completion(url)
            case .orderedDescending, .orderedSame: // 앱스토어 보다 높은 버전 또는 버전이 같은 경우
                return completion(nil)
            }
        }
    }

    //앱 스토어 최신 버전 체크
    private static func getLatestAppStoreVersion(_ completion: @escaping (String?) -> ()) {
        guard let url = URL(string: "http://itunes.apple.com/lookup?id=\(appleID)") else { return completion(nil) }
        
        URLSession.shared.dataTask(with: url) { data, _, _ in
            guard let data else { return completion(nil) }
            
            do {
                guard let json = try JSONSerialization.jsonObject(with: data, options: .allowFragments) as? [String: Any] else { return completion(nil) }
                guard let results = json["results"] as? [[String: Any]] else { return completion(nil) }
                guard let appStoreVersion = results[0]["version"] as? String else { return completion(nil) }
                return completion(appStoreVersion)
            } catch { }
        }.resume()
        return completion(nil)
    }
}
```

AppleID는 < Connect 앱 -> 원하는 앱 선택 -> 앱  정보 > 에서 확인할 수 있다.<br>

getLatestAppStoreVersion: itunes API로 원하는 앱의 정보를 가져온다. 정보 중 버전을 추출해서 completion에 담는다.<br>
updateRequired: getLatestAppStoreVersion에서 가져온 최신 버전과 현재 앱 버전을 비교하고 업데이트가 필요하다면 url를 던져서 view에서 받는다.


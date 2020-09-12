---
title: "&#91;Flutter&#93; Context 없이 Navigation 구현하기"
categories:
  - Front End
tags:
  - flutter
  - dart
toc: true
---

Flutter 글은 처음 작성해본다.

요즘 맨날 Flutter 를 만지는데, context 가 좋다가다도 싫다.

provider 같은 걸 보면 분명 유용한데.. 

오버레이나 네비게이션을 할 때면 무조건 context 가 필요해서 너무 많은 제약이 걸린다.

어쩔 수 없이 비지니스 로직에서 인자를 넘겨 UI 에서 이를 수행하는.. 아름답지 못한 구조가 된다.

나만 싫었던게 아닌건지 Flutter 플러그인들을 보면 Context 없이 하는 서비스를 제공하는 경우가 종종 생기고 있다.

앞으로 이런 플러그인들이 어떤 원리로 돌아가는지 글을 올리려고 한다.

우선 How to navigate without context 에 대해서 알아보자.

## 1. 기본적인 원리

Material App 의 navigator key 라는 것이 있다.

```dart
return MaterialApp(
    title: 'How to navigate without buildcontext',
    debugShowCheckedModeBanner: false,
    navigatorKey: GlobalKey<NavigatorState> navigatorKey
);
```
이 Property 를 주게 되면, 이 key 가 현재 navigation 을 추적하게 된다.

그래서 Navigator.of 를 할 필요 없이, navigatorKey.currentState 로 navigator 를 얻어낼 수 있다.

좀 더 깊게 들어가면 많은 응용법이 숨어있다. onGeneralRoute 라던가.

이런 것은 다음에 작성하도록 하겠다.

여하튼 핵심은 navigatorKey 를 제공하는 것이다. 

이 naviagtorKey 를 관리하고, 이와 관련된 Service 를 제공하는 객체를 구현해보자.

## 2. Service Locator Pattern

내가 아래에서 사용할 패턴은 Service Locator Pattern (서비스 중개자 패턴)이다.

이 패턴은 Service Locator 가 Service 들에 대한 구체 클래스는 숨긴채로 Service 를 제공해주는 패턴이다.

이걸 구현하기 위해서는 우선 Service Locator 가 필요하다. 이건 get_it 플러그인을 사용한다.

그리고 Service Provider 가 필요한데, 이건 **3** 번에서 구현하고 있다.

자세한 설명은 [디자인 패턴 - 서비스 중개자 패턴(Service locator pattern)][2] 를 참고하자

사실 이게 안티패턴이라는 얘기도 있는데.. 아직 잘 이해하진 못했다.

## 3. Service 구현

```dart
abstract class NavigationService {
    get key;

    void pop({Object arguments});

    Future<dynamic> pushNamed(String routeName, {Object arguments});

    Future<dynamic> pushNamedAndRemoveAll(String routeName, {Object arguments});
}
```

```dart
class NavigationServiceImpl implements NavigationService {
    final GlobalKey<NavigatorState> navigatorKey = GlobalKey<NavigatorState>();

    @override
    get key => navigatorKey;

    @override
    void pop({Object arguments}) {
        return navigatorKey.currentState.pop();
    }

    @override
    Future pushNamed(String routeName, {Object arguments}) {
        return navigatorKey.currentState.pushNamed(routeName, arguments: arguments);
    }

    @override
    Future pushNamedAndRemoveAll(String routeName, {Object arguments}) {
        return navigatorKey.currentState.pushNamedAndRemoveUntil(
            routeName, (Route<dynamic> route) => false,
            arguments: arguments);
    }
}
```
이건 내가 실제 사용하고 있는 소스코드의 일부이다.

Service 는 Pattern 에 맞추어 Interface 를 작성하고 구현해주자.

코드 자체는 간단하다. Navigation Service 는 Global Navigation 을 담당할 책임을 가진다.

그래서 NavigatorState Global key 를 멤버로 가지고 있고,

이를 이용한 다양한 Navigation Service 를 제공해준다.

`navigatorKey.currentState` 를 하면 현재 navigtion 을 얻을 수 있고

`navigatorKey.currentState.context` 를 하면 현재 context 를 얻을 수 있다.

참고로 `navigatorKey.currentState.overlay.context` 를 하면 오버레이 할 수 있는 context 를 얻는다.

이걸 이용해서 토스트나 스낵바 등을 만들 수 있다.

## 4. get_it 으로 service locate 하기

[get_it][1] 플러그인은 service locator 플러그인이다.

Locator 를 초기화 하는 함수를 만들어주자

```dart
void setupLocator() {
    locator.registerSingleton<NavigationService>(NavigationServiceImpl());
}
```

이 함수를 `main()` 에서 불러주면 service provider 가 locator 에 등록된다.

그리고 `MaterialApp` 에 service 의 global key 를 제공해주자.

```dart
return MaterialApp(
    title: 'How to navigate without buildcontext',
    debugShowCheckedModeBanner: false,
    navigatorKey: locator<NavigationService>().key
);
```

이를 비지니스 로직에서 사용하는 예제코드는 아래와 같다.

```dart
Future login() async {
    if(povisId = 'hajae' && password = 12345678){
        print('로그인 성공');
        locator<NavigationService>().pushNamedAndRemoveAll('/home');
    }
}
```

이렇게 하면 이제 UI 에서 navigation 을 분리해낼 수 있다!

하지만 그렇다고 모든 navigation 을 옮기는 건 그닥 좋지 않다.

적절히 필요에 맞춰서 사용하자.

## 5. get 플러그인?

[get][3] 플러그인이란 게 있다.

위에서 말한 원리를 이용해서 context 없이 dialog, snackbar, navigation 등을 제공하는 플러그인이다.

정말 유용하긴 하지만.. reddit 같은 곳에서의 반응은 굉장히 싸하다.

Readme 문서부터가 잘못됐다나..

개인적으로는 사용하는 것 자체는 나쁘지 않다고 보지만, 아무 생각없이 그냥 플러그인을 쓰는 건 좋지 않다고 생각한다.

우선 스스로 해당 서비스를 구현해보고, 좀 더 편의를 위해 get 플러그인 도입을 고려해보자.

나도 다음 프로젝트 부터는 get 을 사용해볼까 고민중이다.

[1]: https://pub.dev/packages/get_it
[2]: http://hajeonghyeon.blogspot.com/2017/06/service-locator-pattern.html
[3]: https://pub.dev/packages/get
# RxSwift

## About

Swift версия фреймворка Rx \(Reactive Extensions\). 

По факту для работы с интерфейсом Observable. git: [https://github.com/ReactiveX/RxSwift](https://github.com/ReactiveX/RxSwift)

## Что входит

* RxBlocking
* RxCocoa
* RxRelay
* RxSwift
* RxTest

Взаимодействуют они так

![](../../../.gitbook/assets/izobrazhenie%20%289%29.png)

### Для чего нужны компоненты RxSwift, RxCocoa, RxRelay, ..

* **RxSwift**: The core of RxSwift, providing the Rx standard as \(mostly\) defined by [ReactiveX](https://reactivex.io). It has no other dependencies.
* **RxCocoa**: Provides Cocoa-specific capabilities for general iOS/macOS/watchOS & tvOS app development, such as Binders, Traits, and much more. It depends on both `RxSwift` and `RxRelay`.
* **RxRelay**: Provides `PublishRelay` and `BehaviorRelay`, two [simple wrappers around Subjects](https://github.com/ReactiveX/RxSwift/blob/master/Documentation/Subjects.md#relays). It depends on `RxSwift`.
* **RxTest** and **RxBlocking**: Provides testing capabilities for Rx-based systems. It depends on `RxSwift`.


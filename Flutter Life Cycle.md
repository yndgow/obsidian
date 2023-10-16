# Life Cycle

## 1. Life Cycle 이란?
	앱 또는 프로그램 내의 객체나 컴포넌트가 생성되고 소멸되는 과정을 기술하는 것을 생명주기(Life Cycle)라고 합니다. 특히 UI 프레임워크나 앱 개발에서, 위젯이나 컴포넌트의 생명주기는 매우 중요한 개념입니다.

## 2. StatelessWidget 과 StatefulWidget의 생명 주기
StatelessWidget은 상태를 가지지 않는 위젯입니다. 그래서 생명주기가 간단합니다.
- **Constructor** - StatelessWidget 인스턴스가 생성됩니다.
- **build** - 위젯이 빌드되어 화면에 UI를 표현합니다..


![](https://i.imgur.com/XILzUjM.png)


StatefulWidget은 상태를 가지는 위젯입니다. 그래서 생명주기가 복잡합니다.
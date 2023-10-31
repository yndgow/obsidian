상태 관리를 위해 라이브러리 사용

단, Riverpod을 배우기 위해서는 먼저 Provider 개념을 먼저 이해하는 것이 중요합니다. Provider는 Riverpod의 이전 버전에서 사용되던 라이브러리이며, Riverpod은 Provider의 개념을 기반으로 확장된 라이브러리입니다. 그럼 Provider 만들어진 이유에 대해서 함께 살펴 봅시다.

![](https://i.imgur.com/mRn3I7C.png)


예를 들어 위과 같은 위젯 트리를 구성했다고 가정하고 왼쪽 Stateful 위젯과 오른쪽 stateful 위젯이 상태(데이터, UI, 입력정보 등)를 공유하고 싶다면, 우리는 아래 그림과 같이 위젯을 변경 해야 합니다.

![](https://i.imgur.com/50JuDM1.png)


위 그림 처럼 Lift up 개념을 활용하여 상태를 최상위로 올려야 합니다. 이렇게 되면 Stateful 위젯의 상태 값이 변경될 때마다 상태 값에 영향을 받지 않는 하위 Stateless 위젯 까지 그림이 다시 그려지게 되는 단점이 생깁니다. 결국 전체 그림이 다시 그려지기 때문에 휴대폰의 리소스 낭비가 생기게 됩니다. 그래서 위 단점을 해결하기 상태를 외부에서 관리 하기 위해 사용하는 라이브러리로는 Provider, Riverpod, Bloc/Cubit, Getx 등을 활용 할 수 있습니다. 간단한 이해를 돕기 위해 아래 그림을 살펴 봅시다.

![](https://i.imgur.com/nlu0pBB.png)

Provider는 상태 관리를 위한 강력한 도구로, 상태 값을 외부에서 제공하고 업데이트할 수 있도록 해줍니다. 즉, Provider는 애플리케이션과 위젯 트리 사이에서 동작하며, 상태를 제공하고 상태가 변경될 때 위젯 트리에 업데이트를 전파합니다.

하지만 Provider 는 Boilerplate 코드의 증가, 성능 이슈, 복잡한 의존성 관리, 디버깅의 어려움 등 여러가지 문제 때문에 단점을 보완하고 개발자들이 더 효과적으로 상태를 관리 할 수 있도록 Riverpod 라이브러리가 나오게 되었습니다.

https://docs.flutter.dev/data-and-backend/state-mgmt/options

**1단계 핵심 개념**

**ProviderContainer 창고라고 생각하고 ProviderScope 창고가 제공할 수 있는 범위 그리고 Provider란 창고 관리자라고 생각해 봅시다.**

- **ProviderContainer (창고)** ProviderContainer은 상태 값을 저장하고 관리하는 역할을 합니다. 이것은 상태 값을 포함하는 다양한 Provider를 생성하고 관리하는 창고와 같은 것으로 볼 수 있습니다.
- **ProviderScope (창고가 제공할 수 있는 범위)** ProviderScope는 ProviderContainer가 관리하는 상태 값을 사용할 수 있는 범위를 지정합니다. 특정한 컴포넌트, 페이지 또는 영역 내에서 상태 값을 사용하거나 공유하기 위해 ProviderScope를 사용할 수 있습니다.
- **Provider (창고 관리자)** Provider는 ProviderContainer를 통해 상태 값을 제공하고 관리하는 역할을 합니다. Provider는 해당 창고 (ProviderContainer)에서 상태 값을 외부에 노출하고 필요한 곳에 제공합니다. 이것은 상태 값과 해당 상태 값을 제공하는 함수를 관리하는 역할을 수행합니다.
- **ConsumerWidget (구독자) / ConsumerStatefulWidget (구독자 상태 위젯)** 이러한 위젯은 Riverpod에서 상태를 구독하고 UI를 업데이트하는 데 사용됩니다. 이들은 상태 변화를 감지하고 UI를 업데이트하는 데 도움을 주며, 상태 값을 소비하고 표시하는 역할을 합니다.

작업 순서 
1. flutter_riverpod_01 프로젝트를 생성합니다.
2. flutter_riverpod 라이브러리를 pub.dev 사이트에서 최신 버전을 가져옵니다. 
3. pubspec.yaml 파일을 수정하여 라이브러리를 등록해줍니다. 
4. Todo 클래스 생성 
5. lib/main.dart 파일을 수정합니다.
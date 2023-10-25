
https://docs.flutter.dev/data-and-backend/state-mgmt/intro

## 1. 상태 관리에 대한 이해

플러터에서 상태 관리는 굉장히 중요한 개념입니다

### **1. 상태 (State) 란?**

상태는 앱의 정보나 데이터를 나타내는 것으로, 어떤 시점에서 앱이 "어떻게 보이는지"와 "어떻게 동작하는지"를 결정합니다. 예를 들면, 체크박스의 체크 여부, 텍스트 입력 필드의 내용, 리스트의 아이템 등이 상태에 해당합니다.

### **2. 위젯 (Widget) 이란?**

플러터에서 모든 UI 요소는 위젯입니다. 위젯은 불변(Immutable)합니다. 즉, 한 번 생성되면 변경할 수 없습니다. 그렇기 때문에 UI를 업데이트하기 위해서는 새로운 위젯을 생성해야 합니다.

### **3. 상태 없는 위젯 (StatelessWidget)**

상태를 갖지 않는 위젯으로, 한 번 생성되면 변경되지 않습니다. **`build`** 메서드를 통해 UI를 정의합니다.

### **4. 상태 있는 위젯 (StatefulWidget)**

변경 가능한 상태를 갖는 위젯입니다. **`State`** 객체를 통해 상태를 관리하며, 상태가 변경될 때마다 **`setState`** 메서드를 호출하여 UI를 업데이트합니다.

### **5. 상태 관리 기법**

플러터에서는 다양한 상태 관리 기법과 도구가 있습니다:

- **Local State**: 작은 위젯에서 상태를 관리할 때 유용합니다.
- **Lifting State Up**: 상태를 상위 위젯으로 이동하여 여러 위젯이 상태를 공유할 수 있게 합니다.
- **Provider, Riverpod**: 상태 객체를 효과적으로 제공하고 관리하는 도구입니다.
- **Redux, BLoC, MobX**: 큰 앱에서 복잡한 상태 관리에 사용되는 패턴 및 라이브러리입니다.









구조잡기
```
class Product{  
  String productId;  
  String productName;  
  double price;  
  
  Product(this.productId, this.productName, this.price);  
}
```

```
import 'package:class_my_cart/models/product.dart';  
import 'package:flutter/material.dart';  
  
class ProductList extends StatelessWidget {  
  ProductList({super.key});  
  
  // 샘플 데이터 --> view_model로 옮길 예정  
  List<Product> productList = List.generate(10, (index) => Product('p_$index', '상품 $index', 1000));  
  
  @override  
  Widget build(BuildContext context) {  
    return ListView.builder(  
      itemBuilder: (ctx, index) {  
        return ListTile(  
          leading: Text('${productList[index].productId}'),  
          title: Text('${productList[index].productName}'),  
          trailing: IconButton(  
            icon: Icon(Icons.shopping_cart),  
            onPressed: () {  
              // 로직 추가 예정  
  
            },  
          ),  
        );  
      },  
      itemCount: productList.length,);  
  }  
}
```

```
import 'package:class_my_cart/view/product_list.dart';  
import 'package:flutter/material.dart';  
  
void main() {  
  runApp(MyApp());  
}  
  
class MyApp extends StatefulWidget {  
  const MyApp({super.key});  
  
  @override  
  State<MyApp> createState() => _MyAppState();  
}  
  
class _MyAppState extends State<MyApp> {  
  @override  
  Widget build(BuildContext context) {  
    return MaterialApp(  
      home: SafeArea(  
        child: Scaffold(  
          body: IndexedStack(  
            children: [  
              ProductList(),  
            ],  
          ),  
        ),  
      ),  
    );  
  }  
}
```

```
import 'package:class_my_cart/models/product.dart';  
import 'package:flutter/material.dart';  
  
class MyCart extends StatelessWidget {  
  MyCart({super.key});  
  
  // 샘플 데이터 --> view_model로 옮길 예정  
  List<Product> cartList = List.generate(2, (index) =>  
      Product('p_$index', '상품 $index', 1000)  
  );   
  
  @override  
  Widget build(BuildContext context) {  
    return Column(  
      children: [  
        Row(  
          children: [  
            const Icon(  
              Icons.shopping_cart,  
              size: 30, color: Colors.orangeAccent,  
            ),  
            Text(  
              '${cartList.length} 개',  
              style: const TextStyle(  
                  fontSize: 30,  
                  fontWeight: FontWeight.bold)  
              ,)  
          ],  
        )  
      ],  
    );  
  }  
}
```
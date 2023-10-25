
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




## 2. 코드 작성

### 1. 구조잡기

![](https://i.imgur.com/cIL3ygP.png)

### 2. 코드

- product

```dart
class Product{  
  String productId;  
  String productName;  
  double price;  
  
  Product(this.productId, this.productName, this.price);  
}
```

- my_cart

```dart
import 'package:flutter/material.dart';  
import '../view_models/my_cart_view_model.dart';  
  
class MyCart extends StatelessWidget {  
  final MyCartViewModel myCartVm;  
  MyCart({required this.myCartVm ,super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    return Column(  
      mainAxisAlignment: MainAxisAlignment.center,  
      children: [  
        Row(  
          mainAxisAlignment: MainAxisAlignment.center,  
          children: [  
            const Icon(  
              Icons.shopping_cart,  
              size: 30, color: Colors.orangeAccent,  
            ),  
            Text(  
              '${myCartVm.items.length} 개',  
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

- product_list

```dart
import 'package:flutter/material.dart';  
import '../view_models/my_cart_view_model.dart';  
import '../view_models/product_list_view_model.dart';  
  
class ProductList extends StatefulWidget {  
  
  final ProductListViewModel productVm;  
  final MyCartViewModel myCartVm;  
  
  ProductList({required this.productVm ,super.key, required this.myCartVm});  
  @override  
  State<ProductList> createState() => _ProductListState();  
}  
  
// statefulWidget 으로 변경, 상위클래스, 하위클래스가 존재  
// 하위 클래스에서 --> 상위 클래스에 접근하기 위해 widget을 참조 변수를 제공합니다.  
// 즉 widget은 StatefulWidget 클래스의 인스턴스를 참조하며, 이를 통해 부모 위젯으로부터  
// 데이터를 전달받거나 부모 위젯의 메서드를 호출할 수 있습니다.  
class _ProductListState extends State<ProductList> {  
 // DI 외부에서 생성자를 통해서 데이터를 주입  
  @override  
  Widget build(BuildContext context) {  
    return ListView.builder(  
      itemBuilder: (ctx, index) {  
        return ListTile(  
          leading: Text('${widget.productVm.products[index].productId}'),  
          title: Text('${widget.productVm.products[index].productName}'),  
          trailing: IconButton(  
            icon: Icon(Icons.shopping_cart),  
            onPressed: () {  
              // 로직 추가 예정  
              setState(() {  
                widget.myCartVm.addProduct(widget.productVm.products[index]);  
              });  
            },  
          ),  
        );  
      },  
      itemCount: widget.productVm.products.length,);  
  }  
}
```

- my_cart_view_model

```dart
import '../models/product.dart';  
  
class MyCartViewModel{  
  
  // 데이터 상태 값  
  List<Product> _items = [];  
  List<Product> get items => _items;  
  
  // 아이템 등록 기능  
  void addProduct(Product product){  
    _items.add(product);  
  }  
  
  // 아이템 제거 기능  
  void removeProduct(Product product){  
    _items.remove(product);  
  }  
}
```


- product_list_view_model

```dart
import '../models/product.dart';  
  
class ProductListViewModel{  
  final List<Product> _productList = List.generate(10, (index) => 
	  Product('p_$index', '상품 $index', 1000));  
  List<Product> get products => _productList;  
}
```

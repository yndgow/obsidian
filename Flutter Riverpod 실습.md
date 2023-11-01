```dart
import 'package:flutter/material.dart';  
import 'package:flutter_riverpod/flutter_riverpod.dart';  
  
void main(){  
  runApp(ProviderScope(child: MyApp()));  
}  
  
class Menu{  
  int id;  
  String recipe;  
  Menu(this.id, this.recipe);  
}  
  
final menuProvider = Provider<Menu>((ref) {  
  return Menu(1, "김치찌개");  
});  
  
  
  
class MyApp extends StatelessWidget {  
  const MyApp({super.key});  
  
  @override  
  Widget build(BuildContext context) {  
    return const Placeholder();  
  }  
}  
  
  
class MenuPage extends ConsumerWidget {  
  const MenuPage({super.key});  
  @override  
  Widget build(BuildContext context, WidgetRef ref) {  
      
    Menu menu = ref.read(menuProvider);  
    return Center(  
      child: Text('메뉴 레시피 : ${menu.id}, ${menu.recipe}'),  
    );  
  }  
}
```

![](https://i.imgur.com/NbxnkeA.png)

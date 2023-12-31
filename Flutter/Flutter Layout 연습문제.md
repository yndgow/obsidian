# Flutter Layout 연습문제
**2023.10.13**

### 1. 계산기 화면 만들어보기

![](https://i.imgur.com/ibNXSYw.png)

```dart
import 'package:flutter/material.dart';  
  
void main(){  
  runApp(const MyApp());  
}  
  
class MyApp extends StatelessWidget {  
  const MyApp({super.key});  
  
  @override  
  Widget build(BuildContext context) {  
  
    const calAreaColor = Color(0xff03A9F4);  
    const plusColor = Color(0xffF4D987);  
    const buttonColor = Color(0xff03DAC5);  
    const caColor = Color(0xffF37D7D);  
  
    return MaterialApp(  
      debugShowCheckedModeBanner: false,  
  
      home: Scaffold(  
        appBar: AppBar(title: const Text('Calculator'), backgroundColor: Colors.black),  
        body: Column(  
          children: [  
            Flexible(  
              flex: 1,  
                child: _buildContainerCalResult(calAreaColor, '0')  
            ),  
            Flexible(  
              flex: 2,  
              child: Row(  
                children: [  
                  Expanded(child: _buildContainerNumber(buttonColor, '1')),  
                  Expanded(child: _buildContainerNumber(buttonColor, '2')),  
                  Expanded(child: _buildContainerNumber(buttonColor, '3')),  
                  Expanded(child: _buildContainerNumber(caColor, 'CA')),  
                ],  
              ),  
            ),  
            Flexible(  
              flex: 2,  
              child: Row(  
                children: [  
                  Expanded(child: _buildContainerNumber(buttonColor, '4')),  
                  Expanded(child: _buildContainerNumber(buttonColor, '5')),  
                  Expanded(child: _buildContainerNumber(buttonColor, '6')),  
                  Expanded(child: _buildContainerNumber(plusColor, '+')),  
                ],  
              ),  
            ),  
            Flexible(  
              flex: 2,  
              child: Row(  
                children: [  
                  Expanded(child: _buildContainerNumber(buttonColor, '7')),  
                  Expanded(child: _buildContainerNumber(buttonColor, '8')),  
                  Expanded(child: _buildContainerNumber(buttonColor, '9')),  
                  Expanded(child: _buildContainerNumber(buttonColor, '0')),  
                ],  
              ),  
            ),  
          ],  
        ),  
      ),  
    );  
  }  
}  
  
Container _buildContainerCalResult(Color color, String num){  
  return Container(  
        alignment: Alignment.centerRight,  
        padding: const EdgeInsets.all(15),  
        color: color,  
        child: Text(num, textAlign: TextAlign.end, style: const TextStyle(fontSize: 50, color: Colors.white)),  
      );  
}  
  
Container _buildContainerNumber(Color color, String num){  
  return Container(  
    alignment: Alignment.center,  
    color: color,  
    child: Text(num, style: const TextStyle(fontSize: 30, color: Colors.white)),  
  );  
}
```

![](https://i.imgur.com/pFYpxq6.png)


-  container에 flex를 사용하자
-  color 값을 hex 코드로 줘야하는데 가져올때 \# 을 뺀 6자리를 사용해야한다.
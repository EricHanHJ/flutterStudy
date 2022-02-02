# Null safety
```dart
int? couldBeNull = 1; // null 로 변경가능
couldBeNul = null; // 정상

int couldNotBeNull = 1;
couldNotBeNull = null; // 에러

List<int?> couldBeNullInList = [0, null];
couldBeNullInList[0]!; // !를 넣어서 null이 아님을 표시해야함
couldBeNullInList[1];  // null이기 때문에 !를 넣지않음
```

# Async(1)
```dart
void main() async {         // async : main()은 비동기함수다(함수내부에 await를 쓰면 그 함수는 비동기 함수)
  await getVersionName().then(value) => { // await : 비동기함수 getVersionName()을 호출할테니  진행을 중지하고 대기
    print(value)
  });
  print('end');
}

Future<String> getVersionName() async { // Future : 비동기함수 리턴값(여러개는 Stream), async : getVersionName()은 비동기함수다(함수내부에 await를 쓰면 그 함수는 비동기 함수)
  var versionName = await lookupVersionName(); // await : 리턴값 반환할꺼니 진행을 중지하고 대기
  return versionName;
}

String lookUpVersionName() {
  return 'Android 11';
}
```
#### result :
Android 11   
end   

# Async(2)
```dart
void main(){
  printOne();
  printTwo();
  printThree();
}

void printOne(){
  print('print 1');
}

void printThree(){
  var three = 3;
  print('print $three');
}

void printTwo() async {
  await Future.delayed(Duration(seconds: 1), () {
    print('Future!!');
  });
  print('print 2');
}
```
#### result :
print 1   
print 3   
Future!!     -> await에 의해서 parent(void main())이 종료될때까지 기다린 후 실행   
print 2

# JSON(1)
```dart
void main() {
  var jsonString = '''
  [
    {"score": 40},
    {"score": 80}
  ]
  ''';
  
  var scores = jsonDecode(jsonString);
  print(scores);
```
#### result : 
[{score: 40}, {score: 80}]  -> Maps in List

# JSON(2)
```dart
import 'dart:convert';
void main() {
  var scores = 
  [
    {'score': 40},
    {'score': 80},
    {'score':100, 'overtime':true}
  ];
  
  var jsonText = jsonEncode(scores);
  print(jsonText);
}
```
#### result : 
[{"score":40},{"score":80},{"score":100,"overtime":true}]

# Stream(1)
```dart
import 'dart:async';

Future<int> sumStream(Stream<int> stream) async { // Future : 비동기함수 리턴값, async : sumStream()은 비동기함수다
  var sum = 0;
  await for (var value in stream) { // stream 변수 내 모든 인덱스 게산 끝날때까지 대기해라
    print('sumStream : $value');
    sum += value;
  }
  return sum;
}

Stream<int> countStream(int to) async* { // Stream : 비동기함수 리턴값, async* : countStream()은 비동기함수다
  for (int i=1; i<=to; i++){
    print('countStream : $i');
    yield i; // yield : return값 Stream이 가득찰때까지 누적(async*와 셋트) 
  }
}

main() async{ // main()은 비동기함수다
  var stream = countStream(10);
  var sum = await sumStream(stream); // sumStream(stream) 끝날때까지 대기해라
  print(sum);
}
```
#### result : 
countStream : 1   
sumStream : 1   
countStream : 2   
sumStream : 2   
(중략)   
countStream : 9   
sumStream : 9   
countStream : 10   
sumStream : 10   
55

# Stream(2)
```dart
main() {
  var stream = Stream.fromIterable([1,2,3,4,5]);
  stream.elementAt(0).then((value) => print('first: $value'));
}
```
#### result : 
1

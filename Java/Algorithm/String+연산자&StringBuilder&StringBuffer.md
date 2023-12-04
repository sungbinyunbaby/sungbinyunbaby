# StringBuilder와 String +의 차이

## String +연산자:
문자열을 먼저 String Builder로 변환 후 Append로 문자열을 더하고 다시 toString함수로 문자열로 반환하는 방식.  
즉, 문자열을 합칠 때 +연산자를 사용하면 인스턴스 내의 문자열이 바뀌는 것이 아니다.
새로운 문자열이 담긴 String 인스턴스가 '생성'된다.  
만약 반복문에서 String +연산자를 사용하면
사용한 횟수만큼 StringBuffer 객체가 생성되고 append()와 toString()메서드의 호출이 매번 발생하게 된다.  
이로인해 성능이 저하, 메모리 낭비도 커진다.
(a의 길이 + b의 길이를 가지는 새로운 배열을 만들어와서 a와 b를 모두 복사해 넣고 그걸 c로 쓰는것)

## 이유:
String 변수의 불변성 때문  
String은 최종 클래스(하위클래스없음)이며 변경할 수 없다.  
한번 선언된 String 변수는 그 내용이 절대 변하지 않는다.  
내부적으로 새로운 String 객체를 생성해 text를 붙이기 때문.  
문자열끼리 연결하는 작업 시 내부적으로 많은 여러 작업이 발생하고,
기존 문자열 값의 길이에 추가된 문자열의 크기를 더한 새로운 문자열이 생성된다.  
즉, 중간 문자열 객체가 생성되고 이 문자열은 가비지 컬렉터로 들어간다.(많은 가비지 생성.)  
String의 변경이 많을 때에는 Heap메모리에 많은 가비지 생성

>String + 사용시  
-> StringBuilder의 인스턴스로 생성   
-> AbstractStringBuilder 생성자 호출   
-> append 호출(copy발생)   
-> toString 호출(String 인스턴스가 새롭게 생성, value에 새로운 String객체로 전부 copy해주는 작업 발생)

따라서 많은 더하기 연산이 필요한 경우 처음부터 StringBuilder클래스를 사용하여 문자열을 합치는게 더 좋다.

## String Builder/ String buffer?
사용이유: 
String을 임시 객체사용. String 객체 생성을 방지하고 메모리를 절약하기 위함.
StringBuilder은 미리 일정한 크기의 배열을 잡아두고 거기에 붙여나가는 방식. 변경 가능한 문자열을 만들어 준다.
배열이 가득 찼을 때는 기존보다 2배 더 큰 새로운 배열을 만든다. 
Vector/ArrayList와 동일한 방식. (가변 배열처럼 사용가능)
String 객체 수정으로 인한 임시 가비지 생성을 처리하기 위해 StringBuffr/StringBuilder클래스를 사용할 수 있다.

>StringBufferr과 StringBuilder의 차이점:
StringBuffer은 동기화가 된다.
StringBuilder는 동괴화가 되지 않는다.
멀티스레드 환경에서 자주 변경되는 string은 StringBuffer 사용.
싱글스레드 환경에서는 StrinfgBuilder 사용.(싱글스레드에서는 buffer보다 성능이 뛰어나다.)

## StringBuilder 사용방법
1. StringBuilder stringBuilder = new StringBuilder  
2. stringBuilder.append("문자열").append("문자열")
3. String str = stringBuilder.toString //String을 생성하고 stringBuilder의 값을 넣어준다.  

*String +를 사용했을때 사용되는 방식 그대로 하면된다.  
->StringBuilder(->AtstractStringBuilder)->append()->toString()

https://jinseongsoft.tistory.com/369  
https://onlyfor-me-blog.tistory.com/317

# 정리
String은 불변속성 
StringBuffer/StringBuilder은 변경가능.

+연산을 이용하면 값이 바뀌는게 아니라 String에 있던 값을 버리고 새로운 값을 재할당.
기존의 값은 가비지로 남겨진다.  
변경하지 않는 문자열을 자주 읽어들이는 경우 사용하면 유리하다.  
하지만 변경이 많거나 연산이 일어나는 경우에 사용하면, Heap 메모리에 많은 가비지가 생성된다.  
이는 Heap메모리 부족으로 이어져 프로그램의 성능에 치명적 영향을 미칠 수 있다.
# Stream 
### - 스트림을 사용하는 이유  
배열이나 컬렉션으로 원하는 값을 얻을 때  
for문 도배를 방지하기 위함이라고 생각하면 쉽다.  
많은 코드가 있을 경우 10줄의 for문이 있다고 하면 한줄의 stream코드로 고칠 수 있다.

## 스트림 구조  
1. 선언  
2. 가공 - 중간연산 => n번(여러번) 사용가능
3. 반환 - 최종연산 => 단 하나 사용가능.

 - 선언  
3가지 타입으로 선언 할 수 있다.  
변수일 경우, 컬렉션일 경우, 직접 값을 넣을 경우    
1. 변수일 경우:  
 Stream<T> Stream명(원하는이름)  
 = Arrays.stream(스트림으로 하고 싶은 변수명)  

        참고)  
        Arrays.Stream은 java에서 제공하는  
        Arrays의 클래스에 있는 Stream기능을  
        사용한다는 뜻이다.  
        예제)  
        int[] intArray = {1, 23, 15, 15,  
         35, 55, 13, 1, 2};  
        IntStream intStream =  
        Arrays.stream(intArray);  
        (IntStream은 기본 타입에 특화된 스트림으로 만들어 준다.) 
        

2. 컬렉션일 경우:  
 Stream<T> Stream명 = 리스트명.stream();  

        예제)  
        List<String> nameList = new ArrayList<>();  
        Stream<String> nameListStream = nameList.stream();  
   
 3. 직접 값을 넣어 사용하는 경우:  
 Stream<T> stream명 = Stream.of(value, value...);
 
- 가공(중간연산)  
**n번 수행할 수 있다. 즉, 여러개의 중간연산을 사용할 수 있다.  
1. 중간연산 종류

    filter 
    - boolean형식 true면 리턴.  
     조건에 맞게 필터링.

    map 
    - 원하는 특정 형태로 변환할 수 있다.  
    대소문자 변환해주세요.  
    Integer형으로 변환해주세요.  
    등 이렇게 변경해주세요 라고 생각하면 편하다.
    - maptoInt, maptoDouble, mapToLong으로  
    IntStream, DoubleStream, LongStream으로 변환할 수 있다.
    
    sorted  
    - 스트림 내 요소를 정렬한다.
    - IntStream, DoubleStream과 같은 기본형 특화 스트림의 경우 sorted 메서드에 인자를 넘길 수 없다. 그래서 boxed 메서드를 이용해 객체 스트림으로 변환 후 사용해야 한다.  
    (컬렉션에서는 .boxed()를 사용하지 않는다.)  
    .boxed()  
    .sorted(Comparator.reverseOrder());  
    - sorted(Comparator.reverseOrder()); 는 역정렬이다.

    distinct
    - 중복을 제거한다.  
    기본형의 경우 value로 비교하지만,  
    객체의 경우 object.equals 메서드로 비교한다.

    limit
    - 개수를 제한할 수 있다.  
    .limit(2)

    findFirst
    - 처음 값 가져오기

    skip(배열크기 -1).findFirst
    - 스트림의 마지막 값 가져오기

    max(데이터타입::compare)
    - 최대값

    min(데이터타입::compare)
    - 최소값

    sum, average 등은 배열은 바로 사용가능하지만, 컬렉션의 경우 mapToDouble을 이용해 한번 바꿔준 후 사용해야 한다.

    anyMatch 
    - 하나의 값이라도 조건에 맞으면 true

    noneMatch 
    - 하나의 값도 조건에 맞지 않으면 true

    allMatch 
    - 스트림의 값이 모두 조건에 맞아야 true

- 반환  
최종연산  
한개만 (한번만) 가능.(중간연산은 여러개 가능)

forEach
 - 스트림의 각 요소를 소비하면서 람다를 적용한다. void를 반환한다.

 배열형태를 가져온다
 - .toArray();

 컬렉션의 형태를 가져온다
 - .collect(Collector.toList/toSet/toMap());

값이 하나만 있는 경우
- .get(), .getAsInt()

## 스트림의 단점
1. 디버그가 힘들다.  
한번에 모든 것이 수행되기 때문에 에러가 나면 스트림을 다 뜯어놓고 다시 조립을 해야 한다.
모르는 사람이 스트림 코드를 디버깅할라면 굉장히 힘들다.

2. 재활용 불가능  
한번 사용하면 그 스트림은 다시 실행이 안된다.

3. 속도가 비교적 느리다.

https://wakestand.tistory.com/419

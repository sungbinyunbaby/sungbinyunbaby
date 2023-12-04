# BufferReader과 Scanner

## Scanner
키보드의 입력이 키를 누르는 순간 바로 프로그램에 전달된다.  
띄어쓰기와 개행문자를 경계로 하여 입력값을 인식한다.  
따로 가공할 필요가 없다.(int x = scanner.nextInt())  
Scanner은 지원해주는 메서드가 많고, 사용하기 쉽다.  
버퍼 사이즈 1023 char이기 때문에 많은 입력을 할 경우 성능상 좋지 못한 결과를 불러온다.

## BufferReader
버퍼를 사용하여 읽기를 한다.  
버퍼를 사용하는 입력은 개행 문자가 나타나면 버퍼의 내용을 한번에 프로그램에 전달한다 혹은 버퍼가 가득 차면 전달.  
Scanner은 하드디스크(외부장치)이기 때문에 입출력 생각도 생각보다 오래 걸린다. 바로 외부장치에 이동하는 것보다는
중간에 버퍼를 두어 한번에 묶어 보내는 것이 더 효율적이고 빠른 방법이다.  
Scanner와 달리 개행문자만 경계로 인식한다.  
입력받은 데이터가 'String'으로 고정된다. => 데이터 가공 필요  
but Scanner보다 속도가 빠르다.  
버퍼사이즈는 8192 char. 입력이 많을 때 유리하다.  
동기화가 되어서 (StringBuffer와 동일) 멀티 스레드 환경에서도 안전.

## BufferReader 사용법
1. BufferReader br = new BufferReader(new InputStreamReader(System.in)); //선언
2. String s = br.readLine(); //입력
3. int i = Integer.parseInt(br.readLine())  
//가공 형변환
String으로 리턴 값이 고정되어 있어서, 다른 타입으로 입력을 받고자 한다면 반드시 형변환이 필요하다.  
*예외처리를 반드시 필요로 한다.  
main()에 throws IOExcetio  
혹은 readLine()마다 try/catch문으로 감싸도 된다.

데이터 가공:  
BufferReader를 통해 읽어온 데이터는 개행문자 단위로 나누어진다.  
이를 공백 단위로 데이터를 가공하고자 하면 따로 작업을 해줘야 한다.
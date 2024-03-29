# 정렬
정렬: 데이터를 특정한 기준에 따라 순서대로 나열하는 것.  
별도로 문제에서 정렬함수를 구현하라고 하지 않는다면 표준 정렬 라이브러리를 사용하는 것을 추천.

정렬 라이브러리 성능 측정 방법  
import time 
start_time = time.time()  
원하는 정렬 사용
end_time = time.time()
print("원하는 정렬 성능 측정:", end_time - start_time)
- #### 선택정렬   
    처리되지 않은 데이터 중에서 가장 작은 데이터를 선택해 맨 앞에 있는 데이터와 바꾸는 것을 반복하는 것.

    시간 복잡도: 0(n^2)

    정렬이 어느정도 되어 있어도 하나하나 계속 비교를 해야해서 어떤 경우에도 느리다.

    파이썬 스와프 방법 

    ```Python
    array[i], array[min_index] = array[min_index], array[i]
    ```

- #### 삽입 정렬 
    처리되지 않은 데이터를 하나씩 골라 적절한 위치에 삽입한다.  


    선택 정렬에 비해 구현 난이도가 높은 편이지만, 일반적으로 더 효율적으로 동작한다.

    평균 시간 복잡도: O(n^2)  
    최선의 경우 시간 복잡도: 0(N)  
    자신이 앞의 원소보다 크면 else: break가 있어서 정렬이 어느정도 되어있는 상황이라면 엄청 빠르게 정렬할 수 있다.

- #### 퀵 정렬
    기존 데이터를 설정하고 그 기준보다 큰 데이터와 작은 데이터의 위치를 바꾸는 방법.

    일반적으로 많이 사용되는 정렬 알고리즘 중 하나.

    병합 정렬과 더불어 대부분의 프로그래밍 언어의 정렬 라이브러리의 근간이 되는 알고리즘.

    C언어, java, python도 병합정렬 혹은 퀵정렬을 아이디어를 채택한 하이브리드 알고리즘을 사용한다.

    가장 기본적인 퀵 정렬은 첫 번째 데이터를 기준 데이터(Pivot)로 설정한다. 

    첫 번째 데이터가 5라면 왼쪽에서부터 그 다음 원소들 중 자신보다 큰 원소를 선택하고,  
    오른쪽에서부터 5보다 작은 원소를 찾아 선택한다.  
    만약 왼쪽에서 5보다 큰 7이 있고,  
    오른쪽에서 5보다 작은 4가 있다고 하면 7과 4의 위치를 바꾼다.  
    왼쪽에서 기준 피벗보다 큰 원소만 순차대로 선택, 오른쪽에서 기준 피벗보다 작은 원소만 순차대로 선택하여 계속 반복한다.  
    단, 위치가 엇갈리는 경우 기준피펏(5)와 현재 선택된 기준피벗 보다 작은 데이터의 위치를 서로 변경한다.  
    5 4 2 0 3 1 6 9 7 8 에서  
    왼쪽에서 5보다 큰 원소는 6,  
    오른쪽에서 5보다 작은 원소는 1이다.  
    여기서 두 개의 원소를 바꾸게 되면 큰 원소가 작은 원소보다 앞쪽으로 위치를 바꾸게 된다.  
    이럴경우 1 4 2 0 3 5 6 9 7 8로 기준 피벗과 선택피벗의 위치가 바뀌게 된다.  
    이렇게 하면 기준피벗인 5를 중심으로 왼쪽은 기준피벗 보다 작은 원소들이 모이게 되고, 오른쪽 원소들은 기준피벗 보다 큰 원소들이 모이게 된다.  
    왼쪽과 오른쪽을 기준피벗을 기준으로 데이터를 묶는다 => 분할작업. 왼쪽, 오른쪽이 배열로 나누어진다.  
    그러면 왼쪽 배열에서 다시 첫 번째 원소를 기준피벗으로 두고 퀵 정렬을 시작하고, 오른쪽도 동일하게 진행한다.  
    이렇게 퀵 정렬을 재귀적으로 수행하면 완벽한 정렬이 이루어진다.

    시간 복잡도: 0(N log N)  
    이상적인 경우 분할이 절반씩 일어 난다면 기대할 수 있다.  
    최악의 경우: O(n^2)의 시간 복잡도를 가진다.

- 계수정렬

    특정한 조건이 부합할 때만 사용할 수 있지만 매우 빠르게 동작하는 정렬 알고리즘.

    계수정렬은 데이터의 크기 범위가 제한되어 있어 정수 형태로 표현할 수 있을 때 사용 가능하다.

    데이터의 개수가 N, 데이터(양수) 중 최대값이 K일 때 최악의경우에도 수행시간이 O(N+K)를 보장한다.

    주의해야 할 것은 원소의 범위가 많이 차이가 나지 않아야 한다. 심각한 비효율성을 초래할 수 있다.  
    0,2,3,100이 원소로 있으면 0~100까지의 크기의 리스트가 만들어져야 한다.  
    계수정렬은 동일한 값을 가지는 데이터가 여러 개 등장할 때 효과적으로 사용할 수 있다.











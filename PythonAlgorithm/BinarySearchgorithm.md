# 이진 탐색 알고리즘
: 정렬되어있는 데이터에서 특정한 데이터를 빠르게 탐색할 수 있게 해주는 탐색 알고리즘.
1. 순차탐색
    리스트 안에 있는 특정한 데이터를 찾기 위해 앞에서부터 데이터를 하나씩 확인하는 방법.  
    가장 기본적인 탐색 방법.  
    리스트에서 특정 데이터가 있는지 검사할 때 별다른 말이 없다면 기본적으로 순차탐색을 하면 된다.

2. 이진탐색
    정렬되어 있는 리스트에서 탐색 범위를 절반씩 좁혀가며 데이터를 탐색하는 방법.
    시작점, 끝점, 중간점을 이용하여 탐색 범위를 설정한다.  
    O(nlog)  
    중간점의 값과 찾고자 하는 값을 비교해 작으면 오르쪽(중간점-끝점)은 확인할 필요가 없다.  
    찾고자 하는 값이 중간점 보다 크면 왼쪽은 확인할 필요가 없어진다.  
    만약 중간점 보다 찾는 값이 작다면 끝 점을 중간점 보다 아래로 옮긴다.  
    그러면 탐색 범위가 좁아진다. 다시 중간점을 찾고 그게 찾는 값이 아니라면 위와 같은 방법을 수행한다.  
    계속 수행하다가 중간점과 찾는 값이 같다면 종료.  
    ```python
        # 이진탐색 소스코드 구현(재귀함수)
    def binary_search(array, target, start, end):
        #시작값이 마지막 값보다 더 크면 멈춘다.
        # 리스트 정렬이 되어있어야 하기 때문에 원소가 없다고 친다.
        if start > end:
            return None
        # 중간값 설정
        mid = (start + end) // 2
        if array[mid] == target:
            return mid
        # 중간점의 값보다 찾고자 하는 값이 작은 경우 왼쪽 확인
        elif array[mid] > target:
            return binary_search(array, target, start, mid -1)
        else:
            return binary_search(array, target, mid +1, end)

    # 이진탐색 소스코드 (반복문)
    def binary_search_02(array, target, start, end):
        while start <= end:
            mid = (start + end) // 2
            if array[mid] == target:
                return mid
            elif array[mid] > target:
                start = mid + 1
            else:
                end = mid -1
        return None

    n, target = list(map(int, input().split()))
    array = list(map(int, input().split()))

    # 이진탐색 수행 결과 출력
    result = binary_search(array, target, 0, n -1)
    if result == None:
        print("원소가 존재하지 않습니다.")
    else:
        print(result +1)
    ```

    ### 파이썬 탐색 라이브러리 사용: 값이 특정 범위에 속하는 데이터 개수를 구할 때 사용할 수 있다.  
    bisect 함수 사용  
    from bisect import bisect_left, bisect_right  
    
    ```python
    # count_by_range 값이 [left_value, right_value]인 데이터의 개수를 반환하는 함수
    from bisect import bisect_left, bisect_right
    def count_by_range(a, left_value, right_value):
        right_value = bisect_right(a, right_value)
        left_value = bisect_left(a, left_value)
        return right_value - left_value
    # 배열 선언
    a = [1, 2, 3, 3, 3, 3, 3, 4, 4, 8, 9]
    # 값이 4인 데이터 개수 출력
    print(count_by_range(a, 4, 4))
    # 값이 [-1, 3] 범위에 있는 데이터 개수 출력
    print(count_by_range(a, -1, 3))
    ```

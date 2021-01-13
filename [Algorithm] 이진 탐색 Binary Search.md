# [Algorithm] 이진 탐색 Binary Search

#### 개념

* 정렬되어 있는 리스트가 있을 때,  특정한 값을 찾아내는 알고리즘.

* ##### 시간복잡도 : O(log N)

* 중앙값과 비교해나가면서 가능한 정답의 범의를 절반으로 줄여나가며 가능성 여부를 확인

![image-20210113083641585](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210113083641585.png)



#### 활용방법

* 정답을 구하는 문제

  * A에서 B까지 가는 가장 빠른 시간을 구하는문제 - 최적화

  * 가능 여부를 판별하는 문제로 바꾸기 가능

    

* 사능한지 살펴보는 문제

  * A에서 B까지 x라는 시간으로 이동할 수 있을까 - y/n

    

* A에서 B까지 가는 가장 빠른 시간이 M인 경우

  * M보다 빠른 시간 - 모두 불가능
  * M보다 큰 시간 - 모두 가능



#### 구현

* 재귀로 구현

```java

/**
 * 선형자료구조의 검색방법
 * 1. 순차검색 O[N]
 * 2. 이진검색 O[logN]
 * 3. 해싱기법 O[1]
 */

public class Main_0113_binarySearch {
	
	public static void main(String[] args) {
		int arr[] = {1,2,3,4,5,6,7,8,9};
		int key = 8; //찾고자 하는 데이터
		int l =0; 
		int r =arr.length -1;
		
	}
	
	public static void binarySearch(int[] arr, int l, int r, int key) {
		if( l > r) {  // 종료 파트 
			
			System.out.println("못찾음");
			
		}else { // 반복 파트
			int mid = (l+r) / 2; //중간 index
			
			if(key == arr[mid]) {
				System.out.println("찾음");
			}else if(key < arr[mid]) {
				binarySearch(arr, l, mid-1,key);
			}
			
		}
	}
}


```

* 라이브러리 사용

  ex) Arrays.binarySearcg(arr, key)







## 프로그래머스 입국심사 

### 문제 

링크 : https://programmers.co.kr/learn/courses/30/lessons/43238

### 입국심사

n명이 입국심사를 위해 줄을 서서 기다리고 있습니다. 각 입국심사대에 있는 심사관마다 심사하는데 걸리는 시간은 다릅니다.

처음에 모든 심사대는 비어있습니다. 한 심사대에서는 동시에 한 명만 심사를 할 수 있습니다. 가장 앞에 서 있는 사람은 비어 있는 심사대로 가서 심사를 받을 수 있습니다. 하지만 더 빨리 끝나는 심사대가 있으면 기다렸다가 그곳으로 가서 심사를 받을 수도 있습니다.

모든 사람이 심사를 받는데 걸리는 시간을 최소로 하고 싶습니다.

입국심사를 기다리는 사람 수 n, 각 심사관이 한 명을 심사하는데 걸리는 시간이 담긴 배열 times가 매개변수로 주어질 때, 모든 사람이 심사를 받는데 걸리는 시간의 최솟값을 return 하도록 solution 함수를 작성해주세요.

##### 제한사항

- 입국심사를 기다리는 사람은 1명 이상 1,000,000,000명 이하입니다.
- 각 심사관이 한 명을 심사하는데 걸리는 시간은 1분 이상 1,000,000,000분 이하입니다.
- 심사관은 1명 이상 100,000명 이하입니다.

##### 입출력 예

| n    | times   | return |
| ---- | ------- | ------ |
| 6    | [7, 10] | 28     |

##### 입출력 예 설명

가장 첫 두 사람은 바로 심사를 받으러 갑니다.

7분이 되었을 때, 첫 번째 심사대가 비고 3번째 사람이 심사를 받습니다.

10분이 되었을 때, 두 번째 심사대가 비고 4번째 사람이 심사를 받습니다.

14분이 되었을 때, 첫 번째 심사대가 비고 5번째 사람이 심사를 받습니다.

20분이 되었을 때, 두 번째 심사대가 비지만 6번째 사람이 그곳에서 심사를 받지 않고 1분을 더 기다린 후에 첫 번째 심사대에서 심사를 받으면 28분에 모든 사람의 심사가 끝납니다.

[출처](http://hsin.hr/coci/archive/2012_2013/contest3_tasks.pdf)



### 풀이

```java
import java.util.*;

class Solution {
    public long solution(int n, int[] times) {
        Arrays.sort(times);
        return binerySearch(times, n, times[times.length - 1]);
    }
    
    long binerySearch(int[] times, int n, long max){
        long left = 1, right = max * n;
        long mid = 0;
        long ans = Long.MAX_VALUE;

        while(left <= right){
            mid = (left + right) / 2;
            
            if(isPassed(times, n, mid)){
                ans = ans > mid ? mid : ans;
                right = mid - 1;
            } else {
                left = mid + 1;
            }
        }
        return ans;
    }
    
    boolean isPassed(int[] times, int n, long mid){
        long amount = 0;
        
        for(int i = 0 ; i < times.length ; ++i){
            amount += mid / times[i];
        }
        
        if(amount >= n) return true;
        else return false;
    }
}
```


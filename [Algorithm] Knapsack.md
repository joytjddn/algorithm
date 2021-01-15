# [Algorithm] Knapsack



#### 개념

배낭에 담을 수 있는 **무게의 최댓값(MAX)**이 정해져 있고, **일정 가치(VALUE)**와 **무게(WEIGHT)가 있는 짐(ITEM)**을 배낭에 넣을 때, VALUE의 합이 최대가 되도록 짐을 고르는 방법



* 짐(ITEM)을 쪼갤 수 있는 경우 ->  **분할가능 배낭문제(Fractional Knapsack)** : Greedy(탐욕 기법)
* 짐(ITEM)을 쪼갤 수 없는 경우 ->  **0/1 배낭문제(0/1 Knapsack)** : Dynamic Programming 사용



#### 문제풀이

---

https://www.acmicpc.net/problem/1535

# 백준 1535 안녕

![image-20210115090258824](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210115090258824.png)

![image-20210115090335978](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210115090335978.png)



#### 풀이

![image-20210115095108010](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210115095108010.png)

1만 들어왔을 때 고려, 21 들어왔을 때 고려, 79 들어왔을 때 고려하는 방식을 통해 dp배열을 업데이트한다. 

새롭게 고려하는 항목이 21이라고 했을 때 W[21]이 플러스 되는 부분이다

뒤에서부터 업데이트를 해야 제대로 해결



```
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class MaIn_1535_안녕 {
	public static void main(String[] args) throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int N = Integer.parseInt(br.readLine());
		int energy[] = new int [N]; 
		StringTokenizer st = new StringTokenizer(br.readLine()," ");
		
		for(int i=0; i<N; i++) {
			energy[i]= Integer.parseInt(st.nextToken());
		}
		
		int happiness[] = new int[N];
		
		st = new StringTokenizer(br.readLine()," ");
		
		for (int i = 0; i < N; i++) {
			happiness[i] = Integer.parseInt(st.nextToken());
		}
		
		int dp[]= new int[100];
		int max = 0;
		for (int i = 0; i < N; i++) {
			for(int j=99;j>=0;j--) {
				int index = energy[i]+j;
				if(index<100) {
					dp[index] = Math.max(happiness[i]+dp[j], dp[index]);
				}
			}
		}
		for(int i=0; i<100; i++) {
			if(max<dp[i]) max =dp[i];
		}
		System.out.println(max);
	}
}
```


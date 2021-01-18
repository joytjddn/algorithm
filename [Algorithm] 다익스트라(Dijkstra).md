#  [Algorithm] 다익스트라(Dijkstra)

- 한 정점에서 다른 끝점 까지 가는 최단거리를 구할 때 사용

- 플로이드 워셜 알고리즘은 정점 전부에 관한 정보를 업데이트하여 N^3이란 시간복잡도를 가진다. (=AllPairShortest)

- 그러나, 다익스트라는 시작점을 고정하고 DP배열을 업데이트 하는 방식을 통해 n^2이란 시간복잡도를 가짐.

- 다익스트라에서 최종적으로 나온 DP 배열은 시작점에서 그 점까지의 거리를 의미

   

   

  | NODE | 1    | 2    | 3    | 4    | 5    |
  | ---- | ---- | ---- | ---- | ---- | ---- |
  | DP   | 0    | 3    | 5    | INF  | 8    |

  - 다음 표처럼 시작점을 정해주고 모든 점에 대해 DP를 업데이트하면 최단거리를 구할 수 있음
  - 1에서 3까지의 거리는 5, 1에서 4까지의 거리는 INF를 의미







#### 백준 1753 최단경로

https://www.acmicpc.net/problem/1753

![image-20210118095049964](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210118095049964.png)

- 일반적으로 인접행렬 W 을 이용하여 DP를 업데이트

| 방문노드     | 1            | 2                        | 3    | 4                        | 5    |
| ------------ | ------------ | ------------------------ | ---- | ------------------------ | ---- |
|              | INF          | INF                      | INF  | INF                      | INF  |
| {1}          | 0(자기자신0) | INF                      | INF  | INF                      | INF  |
| {1, 2}       | 0            | 2(MIN(D[2], D[2]+W[2][2] | 3    | INF                      | INF  |
| {1, 2, 3}    | 0            | 2                        | 3    | 7(MIN(D[4], D[3]+W[3][4] | INF  |
| {1, 2, 3, 4} | 0            | 2                        | 3    | 7                        | INF  |

- 그러나 위 문제 같은 경우 인접행렬을 이용하여 업데이트시 메모리 초과 -> 우선순위 큐를 이용한 풀이

   

- /*매 턴마다 방문한 node와 관련된 [list.get(now)만](https://study01do.tistory.com/list.get(now)만) 점검 -> 최소값이 갱신될경우 pq에 추가 -> pq는 

 \* 자동정렬되어 그 턴 중 가장 작은 distance(dp)값을 가지는 node를 앞으로 땡겨줌(즉, distance가 최소인 노드 먼저 방문 가능) 
 \*  
 \*     1   2   3   4   5      pq        push           update 
 \* INF 0  INF INF INF INF   {1, 0}    {2, 2}, {3,3}     {1, 2, 2}, {1, 3, 3} 사용 
 \* INF 0   2   3  INF INF   {2, 2}    {3, 3}, {4, 7}     {2, 3, 5}, {2, 4, 4} 사용 
 \* INF 0   2   3   7  INF   {3, 3}      {4, 7}        {3, 4, 6} 사용 
 \* INF 0   2   3   7  INF   {4, 7} 
 */



```
import java.util.ArrayList;
import java.util.Arrays;
import java.util.PriorityQueue;
import java.util.Scanner;
class D implements Comparable{
	int end;
	int distance;
	D(int end, int distance) {
		this.end = end;
		this.distance = distance;
	}
	@Override
	public int compareTo(D o) {
		return this.distance - o.distance;
	}
}
public class Main {
	public static void main(String[] args) {
		Scanner s = new Scanner(System.in);
		int V = s.nextInt();
		int E = s.nextInt();
		int st = s.nextInt();
		
		ArrayList<ArrayList> list = new ArrayList<>();
		for(int i=0; i<=V; i++)	
			list.add(new ArrayList<>());
		for(int i=0; i<E; i++) {
			int u = s.nextInt();
			int v = s.nextInt();
			int w = s.nextInt();
			list.get(u).add(new D(v, w));
		}
		int distance[] = new int[V+1];
		Arrays.fill(distance, 999999);
		boolean visited[] = new boolean[V+1];
		PriorityQueue pq = new PriorityQueue(); //우선순위 큐에는 업데이트 되는 distance를 넣음. 
		distance[st] = 0;
		pq.add(new D(st, 0));
		
		/*매 턴마다 방문한 node와 관련된 list.get(now)만 점검 -> 최소값이 갱신될경우 pq에 추가 -> pq는 
		 * 자동정렬되어 그 턴 중 가장 작은 distance(dp)값을 가지는 node를 앞으로 땡겨줌(즉, distance가 최소인 노드 먼저 방문 가능)
		 *  
		 *          1      2      3      4     5           pq                push                     update
		 *  INF  0   INF INF INF INF      {1, 0}        {2, 2}, {3,3}          {1, 2, 2}, {1, 3, 3} 사용
		 *  INF  0     2      3   INF INF      {2, 2}        {3, 3}, {4, 7}         {2, 3, 5}, {2, 4, 4} 사용
		 *  INF  0     2      3     7    INF      {3, 3}            {4, 7}                {3, 4, 6} 사용
		 *  INF  0     2      3     7    INF      {4, 7}
		 */
		while(!pq.isEmpty()) {  //사실 while문이 전부
			int now = pq.poll().end;
			if(visited[now]) continue;  //이미 방문한 노드일경우 skip
			visited[now] = true;	
			
			for(D node: list.get(now)) {
				if(distance[node.end]>distance[now]+node.distance) {
					distance[node.end] = distance[now]+node.distance;
					pq.add(new D(node.end, distance[node.end]));
				}
			}
		}
		for(int i=1; i<distance.length; i++) {
			if(distance[i]==999999)
				System.out.println("INF");
			else
				System.out.println(distance[i]);
		}
	}
}
```




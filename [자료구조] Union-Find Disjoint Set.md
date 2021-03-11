# [자료구조] Union-Find: Disjoint Set



#### 유니온 파인드(Disjoint Set)

> 공통 원소가 없는, 즉 "상호 배타적" 인 부분 집합들로 나눠진 원소들에 대한 정보를 저장하고 조작하는 자료구조. 
>
> 서로소 집합 자료구조를 의미.



##### Union-Find 지원 연산

* **초기화** : N 개의 원소가 각각의 집합에 포함되어 있도록 초기화.
* **Union** : 두 원소 a, b가 주어질 때, 이들이 속한 두 집합을 하나로 합치는 연산.
* **Find** : a 가 속한 집합의 대표 번호를 반환.



![image-20210311081621478](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210311081621478.png)

##### 시간복잡도

일반적으로 Union-find를 사용하게 될 경우, 평균적으로 트리의 높이만큼 탐색하는 O(logN)이지만, 트리를 형성하는 과정에서 사향트리가 될 수 있으며, 이렇게 될 경우, 시간복잡도는 O(n)이 되어버린다.

이 때, 항상 높이가 더 낮은 트리를 높은 트리 밑에 넣음 으로서 해결이 가능하다. 이렇게 되면 트리의 높이가 크게 높아지는 현상을 방지할 수 있다. 이러한 최적화를 `union-by-rank` 라 하며, 여기서 rank 는 해당 트리의 높이를 저장한다.

> 일반적인 배열로 접근하면 Union 연산은 대표 번호로 최대 N개의 수를 변경해야하기 때문에 O(N)이고, Find 연산은 대표번호를 알 수 있기 때문에 O(1)이다.
>
> **트리로 구현하면** Union 연산은 Find 연산을 이용하여 구현할 수 있고, Find 연산을 최악의 경우 O(N)이지만, 실제로  O(a(N))의 시간복잡도를 가진다. 
>
> a(N)은 아커만 함수로 N이 2^65536일때, 함수 값이 5가 된다. 따라서 상수로 보아도 무관하기에
>
> o(1)로 취급될 정도로 빠르다.

* Find : O(a(N)) => O(1)
* Union : O(a(N)) => O(1)



#### Union-Find 구현

트리로 ``Union-Find``를 구현하면 배열보다 빠르게 Union 연산을 수행할 수 있다.

![image-20210311094730723](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210311094730723.png)

![image-20210311094806438](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210311094806438.png)



#### 소스코드 구현

```java
/**
 * 상호 베타 집합 => MST (크루스칼 알고리즘에 활용)
 * makeSet(int x) 멤버 x를 포함하는 새로운 집합을 생성
 * findSet(int x) 멤버 x를 포함하는 대표자를 리턴
 * union(int x, int y) 멤버 x, y의 대표자를 구해서 두 집합을 통합
 * link(int px, int py) x의 대표자의 집합과 y의 대표자의 집합을 합침
 */
public class Z30_DisjointSets {
	public static int p[] = new int[10]; // 부모를 저장할 배열
	public static int rank[] = new int[p.length]; // rank를 저장할 배열

	public static void main(String[] args) {
		for (int i = 0; i < p.length; i++)
		{
			makeSet(i);
		}
		
		printSet();
		union(0, 1); printSet();
		union(2, 3); printSet();
		union(0, 3); printSet();
		
	}
	/**
	 * 멤버 x를 포함하는 새로운 집합을 생성
	 */
	public static void makeSet(int x)
	{
		p[x] = x; // 부모 : 자신의 index로 표시 or -1;
		//rank[x] = 0; // 초기값 0 자바는 default value가 0이기때문에 생략가능
	}
	
	/**
	 * 멤버 x를 포함하는 대표자를 리턴
	 * @return 대표자
	 */
	public static int findSet(int x)
	{
		if (x == p[x])
			return x;
		else
		{
			p[x] = findSet(p[x]); // path compression
			//rank[x] = 1; 사실 할 필요가 없음. 대표자의 랭크만 알면 되기때문.
			return p[x];
		}
	}
	
	/**
	 * 멤버 x, y의 대표자를 구해서 두 집합을 통합
	 */
	public static void union(int x, int y)
	{
		int px = findSet(x);
		int py = findSet(y);
		
		if (px != py) // 같은 집합을 합치는 경우 문제생기는것을 방지
		{
//			p[py] = px;
			link(px, py);
		}
	}
	
	/**
	 * 두 집합을 합치면서 rank를 최소한으로 합침.
	 */
	public static void link(int px, int py)
	{
		if (rank[px] > rank[py])
		{
			p[py] = px; // 작은 쪼글 큰쪽에 붙인다.
		}
		else
		{
			p[px] = p[py];
			if (rank[px] == rank[py]) // 두 집합의 랭크가 같은 경우는 랭크 값이 증가한다.
				rank[py]++; // 집합의 대표자 랭크가 증가됨
		}
	}
	
	/**
	 * p배열 출력
	 */
	public static void printSet()
	{
		System.out.print("index  : ");
		for (int i = 0; i < p.length; i++)
		{
			System.out.printf("%2d ", i);
		}
		System.out.print("\nparent : ");
		for (int i = 0; i < p.length; i++)
		{
			System.out.printf("%2d ", p[i]);
		}
		System.out.println("\n");
	}
}

```


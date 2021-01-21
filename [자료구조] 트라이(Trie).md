# [자료구조] 트라이(Trie) 



##### 개념

문자열에서 검색을 빠르게 도와 주는 트리 형태의 자료구조

```
정수형에서 이진탐색트리를 이용하면 시간복잡도 O(logN)
하지만 문자열에서 적용했을 때, 문자열 최대 길이가 M이면 O(M*logN)이 된다.

트라이를 → O(M)으로 문자열 검색이 가능
```

 

![트라이 구조](C:\Users\hw030\OneDrive\문서\GitHub\algorithm\트라이 구조.jpg)

마지막 끝나는 노드의 개수를 모두 확인하면 배열의 총 문자열 개수가 된다.

----



## 백준 전화번호 목록(Trie)

https://www.acmicpc.net/problem/5052

##### 문제

![image-20210121093235747](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210121093235747.png)

##### 풀이

이 문제는 해쉬로도 구현이 가능한 문제지만 trie를 통해 구현해 보았다.



trie 구현

```java
public static class Trie{
		boolean end;
		boolean pass;
		Trie[] child;
		
		Trie(){
			end =false;
			pass = false;
			child = new Trie[10];//번호길이 최대 10
		}
		
		public boolean insert(String str, int idx){
			//끝나는 단어 있으면 false 종료
			if(end) return false;
			
			//idx가 str 만클 왔을때
			if(idx == str.length()) {
				end =true;
				if(pass) return false; //더 지나가는 단어 있으면 false
				else return true;
			}
			//아직 안왔을 때
			else {
				int next = str.charAt(idx)-'0';
				if(child[next] == null) {
					child[next] = new Trie();
					pass =true;
				}
				return child[next].insert(str,  idx+1);
			}
		}
	}
```





##### 전체풀이

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main_5052_전화번호목록 {
	
	public static class Trie{
		boolean end;
		boolean pass;
		Trie[] child;
		
		Trie(){
			end =false;
			pass = false;
			child = new Trie[10];//번호길이 최대 10
		}
		
		public boolean insert(String str, int idx){
			//끝나는 단어 있으면 false 종료
			if(end) return false;
			
			//idx가 str 만클 왔을때
			if(idx == str.length()) {
				end =true;
				if(pass) return false; //더 지나가는 단어 있으면 false
				else return true;
			}
			//아직 안왔을 때
			else {
				int next = str.charAt(idx)-'0';
				if(child[next] == null) {
					child[next] = new Trie();
					pass =true;
				}
				return child[next].insert(str,  idx+1);
			}
		}
	}
	
	
	public static void main(String[] args) throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		
		int T=Integer.parseInt(br.readLine());
		for(int tc = 1; tc<=T; tc++) {
			int n = Integer.parseInt(br.readLine());//전화번호 수
			//각 목록마다 일관성을 확인 -> 번호 자체가 접두어인 경우 x
			//즉 모든 번호가 마지막 노드로 존재해야 한다.
			String [] list = new String[n];
			Trie trie = new Trie();
			for(int i=0; i<n; i++) {
				list[i]=br.readLine();
			}
			Arrays.sort(list); //접두어를 확인하기 위해 정렬해서 넣는 편이 더 수월
			
			boolean ans =true;
			
			
			for(int i=0; i<n;i++) {
				if(!trie.insert(list[i], 0)) {
					ans =false;
					break;
				}
			}
			System.out.println(ans ? "YES" : "NO");
			
		}
		
	}
	
}

```


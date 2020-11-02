# 프로그래머스 k번째 수

![image-20201020204324990](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20201020204324990.png)



![image-20201020204341396](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20201020204341396.png)







## 풀이

처음에 문제를 접하고 배열을 자르는 부분을 string 으로 변환 후 자르려고 했다.

예시 테스트케이스는 맞았지만 2번과 6번 테스트케이스가 틀려서

이상하다 생각하고 다른 사람들 풀이를 찾아봤는데 배열을 그대로 잘라서 쓰는 것을 보고 코드를 바꿨다.



이 문제의 경우, 굳이 string으로 변환하여 자를 필요가 없는 문제인 것 같다.







### 첫번째 풀이

```
import java.util.*;

class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int[] answer = {};
        answer= new int[commands.length];
        
        //k번째 수를 구하라
        String s= Arrays.toString(array);
        s=s.substring(1,s.length()-1).replace(", ","");  // 이거 다시 확인
        System.out.println(s);
        for(int tc=0; tc<commands.length; tc++){
            int i=commands[tc][0];
            int j=commands[tc][1];
            int k=commands[tc][2];    
            String temp=s.substring(i-1,j); 
            // System.out.println(temp);
            int []arr= new int[temp.length()];
            for(int index=0; index<arr.length;index++){
                arr[index]=temp.charAt(index)-'0';
            }
            Arrays.sort(arr);
            System.out.print(Arrays.toString(arr));
            // System.out.print(Arrays.toString(arr));
            answer[tc]=arr[k-1];
        }
        return answer;
    }
    
}
```



### 두번째 풀이

```
import java.util.*;

class Solution {
    public int[] solution(int[] array, int[][] commands) {
        int[] answer = {};
        answer= new int[commands.length];
        
        //k번째 수를 구하라
        for(int tc=0; tc<commands.length; tc++){
            int i=commands[tc][0]-1;
            int j=commands[tc][1]-1;
            int k=commands[tc][2]-1;     
            // System.out.println(temp);
            int []arr= new int[j-i+1];
            for(int index=0; index<arr.length;index++){
                arr[index]=array[index+i];
            }
            Arrays.sort(arr);
            System.out.print(Arrays.toString(arr));
            // System.out.print(Arrays.toString(arr));
            answer[tc]=arr[k];
        }
        return answer;
    }
    
}
```


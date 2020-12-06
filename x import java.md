```
import java.util.*;

class Solution {
    public ArrayList<Character>list;
    public int result=0;
    public boolean visited[];
    public int solution(int[] numbers, int target) {

        int answer = 0;
        visited=new boolean[numbers.length];
        list=new ArrayList<Character>();
        int arr[] = new int[numbers.length];
        for(int i=0;i<=arr.length; i++){
            permutation(0,i,numbers, arr,target,0);
        }
        answer=result;
        return answer;
    }
    public void permutation(int k,int n, int[]numbers, int[] arr, int target){
        if(k==n){//k는 -의 개수
            int sum=0;
            for(int i=0; i<numbers.length; i++){
                if(arr[i]==0){
                    sum+=numbers[i];
                }else{
                    sum-=numbers[i];
                } 
            }
            if(sum==target) result++;
            return;
        }
        
        
        for(int i=0; i<arr.length;i++){
            if(!visited[i]){
                visited[i]=true;
                arr[i]=1;
                permutation(k+1,n,numbers,arr,target);
                visited[i]=false;
                arr[i]=0;
            }
        }
    }
}
```


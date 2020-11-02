# 백준 토마토2 - BFS



```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;

public class Main_토마토 {
	private static int h;
	private static int n;
	private static int m;
	private static int[][][] map;
	private static Queue<tomato> q;
	
	private static int[] dx= {1,-1,0,0,0,0};
	private static int[] dy= {0,0,1,-1,0,0};
	private static int[] dz= {0,0,0,0,1,-1};
	private static int day;
	private static boolean inRange(int x,int y, int z) {
		if(x>=0&&x<n&&y>=0&&y<m&&z>=0&&z<h) return true;
		return false;
	}
	public static class tomato{
		int x,y,z;
		
		public tomato(int x,int y,int z) {
			this.x=x;
			this.y=y;
			this.z=z;
		}
		
		
	}
	
	public static void bfs() {
		tomato t = q.poll();
		int x=t.x;
		int y=t.y;
		int z=t.z;
		
		for(int i=0; i<6;i++) {
			int nx = x + dx[i];
			int ny = y + dy[i];
			int nz = z + dz[i];
			if(inRange(nx,ny,nz)&&map[nz][nx][ny]==0) {
				map[nz][nx][ny]=map[z][x][y]+1;
				day=Math.max(day,map[nz][nx][ny]);
				q.add(new tomato(nx,ny,nz));
			}
		}
		
	}
	
	
	public static void main(String[] args) throws Exception {
		BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
		String str[] = br.readLine().split(" ");
		m=Integer.parseInt(str[0]);
		n=Integer.parseInt(str[1]);
		h=Integer.parseInt(str[2]);
		q=new LinkedList<>();
		
		map = new int[h][n][m];
		day=0;
		for(int i=0; i<h; i++) {
			for(int j=0; j<n; j++) {
				str = br.readLine().split(" ");
				for(int k=0; k<m;k++) {
					map[i][j][k]= Integer.parseInt(str[k]);
					if(map[i][j][k]!=0&&map[i][j][k]!=-1) {
						q.add(new tomato(j,k,i));//x,y,z여기였네
					}
				}
			}
		}
		
		while(!q.isEmpty()) {
			bfs();
		}
		
		for(int i=0; i<h; i++) {
			for(int j=0; j<n; j++) {
				for(int k=0; k<m;k++) {
					if(map[i][j][k]==0) {
						System.out.println(-1);
						return;
					}
				}
			}
		}
		if(day!=0) {			
			System.out.println(day-1);
		}else {
			System.out.println(day);
		}
		
		
	}
}
```


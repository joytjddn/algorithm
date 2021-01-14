# [자료구조] 이진탐색트리 (Binary Search Tree)



* 트리 용어 정리

![image-20210114084100143](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210114084100143.png)

#### 개념

##### 이진 탐색트리의 목적 -> 이진탐색 + 연결리스트

이진탐색 : **탐색에 소요되는 시간 복잡도 -> O(logN)**, 삽입, 삭제 불가

연결리스트 : **삽입, 삭제의 시간복잡도 -> O(1)**, 탐색의 시간복잡도 -> **O(N)**



**--> 효율적 탐색 + 삽입, 삭제 가능한 탐색 방법**





![image-20210114083147938](C:\Users\hw030\AppData\Roaming\Typora\typora-user-images\image-20210114083147938.png)

#### 특징

* 각 노드의 자식이 2개 이하인 트리
* 왼쪽 자식은 부모보다 작고, 오른쪽 자식은 부모보다 크다
* 중복된 노드가 없어야 한다-> 검색 목적 -> 중복이 많을 경우 속도가 느려짐 -> 노드에 count값을 가지게 처리하는 것이 더 효율적



```java
public class binarySearchTree {
	
	public class Node {
		private int data;
		private Node left;
		private Node right;
		
		public Node(int data) {
			this.setData(data);
			setLeft(null);
			setRight(null);
		}

		public int getData() {
			return data;
		}

		public void setData(int data) {
			this.data = data;
		}

		public Node getLeft() {
			return left;
		}

		public void setLeft(Node left) {
			this.left = left;
		}

		public Node getRight() {
			return right;
		}

		public void setRight(Node right) {
			this.right = right;
		}
	}
	
	public Node root;
	public binarySearchTree() {
		this.root = null;
	}
	
	//탐색 연산
	public boolean find(int id){
		Node current = root;
		while(current!=null){
			//현재 노드와 찾는 값이 같으면
			if(current.getData()==id){
				return true;
				//찾는 값이 현재 노드보다 작으면
			} else if(current.getData()>id){
				current = current.getLeft();
				//찾는 값이 현재 노드보다 크면
			} else{
				current = current.getRight();
			}
		}
		return false;
	}
	//삭제 연산
	public boolean delete(int id){
		Node parent = root;
		Node current = root;
		boolean isLeftChild = false;
		while(current.getData()!=id){
			parent = current;
			if(current.getData()>id){
				isLeftChild = true;
				current = current.getLeft();
			}else{
				isLeftChild = false;
				current = current.getRight();
			}
			if(current==null){
				return false;
			}
		}
		//Case 1: 자식노드가 없는 경우
		if(current.getLeft()==null && current.getRight()==null){
			if(current==root){
				root = null;
			}
			if(isLeftChild==true){
				parent.setLeft(null);
			}else{
				parent.setRight(null);
			}
		}
		//Case 2 : 하나의 자식을 갖는 경우
		else if(current.getRight()==null){
			if(current==root){
				root = current.getLeft();
			}else if(isLeftChild){
				parent.setLeft(current.getLeft());
			}else{
				parent.setRight(current.getLeft());
			}
		} else if(current.getLeft()==null){
			if(current==root){
				root = current.getRight();
			}else if(isLeftChild){
				parent.setLeft(current.getRight());
			}else{
				parent.setRight(current.getRight());
			}
		}
		//Case 3 : 두개의 자식을 갖는 경우
		else if(current.getLeft()!=null && current.getRight()!=null){
			// 오른쪽 서브트리의 최소값을 찾음
			Node successor = getSuccessor(current);
			if(current==root){
				root = successor;
			}else if(isLeftChild){
				parent.setLeft(successor);
			}else{
				parent.setRight(successor);
			}			
			successor.setLeft(current.getLeft());
		}		
		return true;		
	}

	public Node getSuccessor(Node deleleNode){
		Node successsor =null;
		Node successsorParent =null;
		Node current = deleleNode.getRight();
		while(current!=null){
			successsorParent = successsor;
			successsor = current;
			current = current.getLeft();
		}
		if(successsor!=deleleNode.getRight()){
			successsorParent.setLeft(successsor.getRight());
			successsor.setRight(deleleNode.getRight());
		}
		return successsor;
	}

	//삽입 연산
	public void insert(int id){
		Node newNode = new Node(id);
		if(root==null){
			root = newNode;
			return;
		}
		Node current = root;
		Node parent = null;
		while(true){
			parent = current;
			if(id < current.getData()){				
				current = current.getLeft();
				if(current==null){
					parent.setLeft(newNode);
					return;
				}
			}else{
				current = current.getRight();
				if(current==null){
					parent.setRight(newNode);
					return;
				}
			}
		}
	}
	
	public void display(Node root){
		if(root!=null){
			display(root.getLeft());
			System.out.print(" " + root.getData());
			display(root.getRight());
		}
	}

	public static void main(String[] args) {
		
		binarySearchTree b = new binarySearchTree();
		//트리에 노드를 삽입
		b.insert(3);b.insert(8);
		b.insert(1);b.insert(4);b.insert(6);b.insert(2);b.insert(10);b.insert(9);
		b.insert(20);b.insert(25);b.insert(15);b.insert(16);
		
		System.out.println("트리삽입 결과 : ");
		b.display(b.root);
		System.out.println("");
		System.out.println("이진트리에서 4를 탐색 : " + b.find(4));
		System.out.println("이진트리에서 2를 삭제 : " + b.delete(2));		
		b.display(b.root);
		System.out.println("\n이진트리에서 4를 삭제 : " + b.delete(4));		
		b.display(b.root);
		System.out.println("\n이진트리에서 10을 삭제 : " + b.delete(10));		
		b.display(b.root);
	}

}
```


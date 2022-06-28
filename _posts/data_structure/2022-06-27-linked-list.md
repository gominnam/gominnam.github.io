---
title: 자료구조 - Linked List
date: 2022-06-28 01:26:00 +09:00
categories: [Study, Linked List]
tags: [data structure, linked list]
---

`링크드 리스트(Linked List)`는 추상적 자료형인 리스트를 구현한 자료구조로 동적으로 크기가 변할 수 있고 
삭제나 삽입 시에 데이터를 이동할 필요가 없는 연결된 표현이다. 이 연결은 `포인터를 사용하여 데이터들을 연결`한다.
이 연결된 표현은 사실 추상 데이터 타입 `리스트`의 구현에만 사용되는 것이 아니고 여러 가지의 자료구도(트리, 그래프, 스택, 큐)
 등을 구현하는데도 많이 사용된다.

![linkedlist](/assets/img/posts/data-structure/2022-06-28-linked-list-1.png){: width="200" height="400"}<br>

위 그림에서 1, 2, 3, 4, 5 숫자는 데이터를 넣고 연결된 표현은 숫자 옆의 줄로 연결됐다고 생각하면 되겠다.<br>
링크드 리스트는 데이터를 한군데 모아두는 것을 포기하고 메모리상의 어디에나 흩어져 존재하게 되는데 연결하는 줄(pointer)로 묶음으로써 리스트를 구현한다.<br><br><br>

## 링크드 리스트 구조

![linkedlist](/assets/img/posts/data-structure/2022-06-28-linked-list-2.png){: width="400" height="200"}<br><br>

연결 리스트는 위 그림과 같이 노드(node)들의 집합이다. 노드들은 메모리의 어떤 위치에나 있을 수 있으며 다른 노드로 가기 위해서는 현재 노드가 가지고 있는 포인터를 이용하면 된다.
노드는 위 그림과 같이 데이터 필드(data)와 링크 필드(next)로 구성되어 있다. `데이터 필드에는 저장하고 싶은 데이터가 들어가고, 링크 필드에는 다른 노드를 가리키는 포인터가 저장`된다.
따라서 연결 리스트마다 첫 번째 노드를 가리키고 있는 변수가 필요한데 이것을 헤드 포인터(head pointer)라고 한다.
마지막 노드의 링크 필드는 NULL으로 설정되는데 이는 더 이상 연결된 노드가 없다는 것을 의미한다.
<br><br><br>

## 링크드 리스트 특징

<ul>
    <li>배열보다 데이터를 중간에 삽입, 삭제시 더 용이하다.</li>
    <li>상대적으로 구현이 더 어렵고 오류가 나기 쉽다.</li>
    <li>데이터뿐만 아니라 포인터도 저장하여야 하므로 메모리 공간을 많이 사용한다.</li>
</ul>
<br><br><br>

## 링크드 리스트 종류

- 단순 연결 리스트(singly linked list)는 하나의 방향으로만 연결되어 있는 연결 리스트이다.<br><br>
![linkedlist](/assets/img/posts/data-structure/2022-06-28-linked-list-3.png){: width="400" height="200"}<br><br>

- 원형 연결 리스트(circular linked list)는 단순 연결 리스트와 같으나 마지막 노드의 링크가 첫 번째 노드를 가리킨다.<br><br>
![linkedlist](/assets/img/posts/data-structure/2022-06-28-linked-list-4.png){: width="400" height="200"}<br><br>

- 이중 연결 리스트(doubly linked list)는 각 노드마다 2개의 링크가 존재한다.<br><br>
![linkedlist](/assets/img/posts/data-structure/2022-06-28-linked-list-5.png){: width="400" height="200"}<br><br>

## Source Code

단순연결리스트 구현 소스코드이다.

```java
public class LinkedList{
	Node header;

	public static class Node{
		int data;
		Node next;
	}

	//initialization	**header
	LinkedList(){
		header = new Node();
	}

	//append Node
	public void append(int data){
		Node end = new Node();
		end.data = data;
		Node n = header;

		while(n.next != null){
			n = n.next;
		}

		n.next = end;
	}

	//delete Node
	public void delete(int data){
		Node n = header;

		while(n.next != null){
			if(n.next.data == data){
				n.next = n.next.next;
			}
			else {
				n = n.next;
			}
		}
	}

	//print LinkedList datas
	public void retrieve(){
		Node n = header;

		while(n.next != null){
			n = n.next;
			System.out.println(n.data);
		}

		System.out.println(n.data);
	}

	//double pointer
	public void removeDups(){
		Node n1 = header;

		while(n1 != null && n1.next != null){
			Node n2 = n1;

			while(n2.next != null){
				if(n1.data == n2.next.data){
					n2.next = n2.next.next;
				}
				else{
					n2 = n2.next;
				}
			}

			n1 = n1.next;
		}
	}
}
```

## 참고자료

- <https://namu.wiki/w/%EC%97%B0%EA%B2%B0%20%EB%A6%AC%EC%8A%A4%ED%8A%B8>
- <https://www.andrew.cmu.edu/course/15-121/lectures/Linked%20Lists/linked%20lists.html#:~:text=The%20entry%20point%20into%20a,is%20a%20dynamic%20data%20structure.>
- <https://www.programiz.com/dsa/linked-list-types>
- C언어로 쉽게 풀어쓴 자료구조 - 생능출판
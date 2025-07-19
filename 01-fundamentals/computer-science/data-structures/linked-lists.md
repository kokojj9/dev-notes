# 연결 리스트 (Linked Lists)

## 📚 개요

연결 리스트는 노드들이 포인터로 연결된 선형 자료구조입니다.

## 🎯 학습 목표

- 연결 리스트의 기본 개념 이해
- 단일 연결 리스트와 이중 연결 리스트 구현
- 연결 리스트 관련 알고리즘 문제 해결

## 📖 주요 내용

### 연결 리스트의 특징

- **구조**: 노드들이 포인터로 연결
- **메모리**: 연속되지 않은 메모리 공간 사용
- **접근**: 순차 접근만 가능 (인덱스 접근 불가)
- **크기**: 동적 크기 조정 가능

### 연결 리스트의 종류

1. **단일 연결 리스트 (Singly Linked List)**
2. **이중 연결 리스트 (Doubly Linked List)**
3. **원형 연결 리스트 (Circular Linked List)**

## 💻 구현 예제

### 노드 클래스

```java
// 단일 연결 리스트 노드
class Node {
    int data;
    Node next;

    public Node(int data) {
        this.data = data;
        this.next = null;
    }
}

// 이중 연결 리스트 노드
class DoublyNode {
    int data;
    DoublyNode next;
    DoublyNode prev;

    public DoublyNode(int data) {
        this.data = data;
        this.next = null;
        this.prev = null;
    }
}
```

### 단일 연결 리스트

```java
public class LinkedList {
    private Node head;
    private int size;

    public LinkedList() {
        this.head = null;
        this.size = 0;
    }

    // 노드 추가
    public void append(int data) {
        Node newNode = new Node(data);

        if (head == null) {
            head = newNode;
        } else {
            Node current = head;
            while (current.next != null) {
                current = current.next;
            }
            current.next = newNode;
        }
        size++;
    }

    // 맨 앞에 노드 추가
    public void prepend(int data) {
        Node newNode = new Node(data);
        newNode.next = head;
        head = newNode;
        size++;
    }

    // 노드 삭제
    public boolean delete(int data) {
        if (head == null) return false;

        if (head.data == data) {
            head = head.next;
            size--;
            return true;
        }

        Node current = head;
        while (current.next != null && current.next.data != data) {
            current = current.next;
        }

        if (current.next != null) {
            current.next = current.next.next;
            size--;
            return true;
        }

        return false;
    }

    // 리스트 출력
    public void print() {
        Node current = head;
        while (current != null) {
            System.out.print(current.data + " -> ");
            current = current.next;
        }
        System.out.println("null");
    }

    // 크기 반환
    public int size() {
        return size;
    }

    // 비어있는지 확인
    public boolean isEmpty() {
        return head == null;
    }
}
```

## 🔍 주요 알고리즘

### 1. 연결 리스트 순회

```java
public static void traverse(Node head) {
    Node current = head;
    while (current != null) {
        System.out.println(current.data);
        current = current.next;
    }
}
```

### 2. 연결 리스트 뒤집기

```java
public static Node reverse(Node head) {
    Node prev = null;
    Node current = head;
    Node next = null;

    while (current != null) {
        next = current.next;
        current.next = prev;
        prev = current;
        current = next;
    }

    return prev;
}
```

### 3. 사이클 감지 (Floyd's Cycle Finding)

```java
public static boolean hasCycle(Node head) {
    if (head == null || head.next == null) {
        return false;
    }

    Node slow = head;
    Node fast = head.next;

    while (slow != fast) {
        if (fast == null || fast.next == null) {
            return false;
        }
        slow = slow.next;
        fast = fast.next.next;
    }

    return true;
}
```

### 4. 중간 노드 찾기

```java
public static Node findMiddle(Node head) {
    if (head == null) return null;

    Node slow = head;
    Node fast = head;

    while (fast != null && fast.next != null) {
        slow = slow.next;
        fast = fast.next.next;
    }

    return slow;
}
```

## 📝 연습 문제

### 쉬운 문제

1. 연결 리스트의 길이 구하기
2. 연결 리스트에서 특정 값 찾기
3. 연결 리스트의 중간 노드 찾기

### 중간 문제

1. 연결 리스트 뒤집기
2. 두 연결 리스트 합치기
3. 연결 리스트에서 중복 제거

### 어려운 문제

1. 연결 리스트에서 사이클 감지
2. 연결 리스트 정렬
3. 연결 리스트에서 k번째 노드 제거

## 🛠️ 구현 팁

### 더미 노드 활용

```java
public class LinkedListWithDummy {
    private Node dummy;
    private int size;

    public LinkedListWithDummy() {
        this.dummy = new Node(0); // 더미 노드
        this.size = 0;
    }

    public void append(int data) {
        Node newNode = new Node(data);
        Node current = dummy;

        while (current.next != null) {
            current = current.next;
        }
        current.next = newNode;
        size++;
    }

    public Node getHead() {
        return dummy.next;
    }
}
```

### 재귀적 접근

```java
public static Node reverseRecursive(Node head) {
    if (head == null || head.next == null) {
        return head;
    }

    Node newHead = reverseRecursive(head.next);
    head.next.next = head;
    head.next = null;

    return newHead;
}
```

### 이중 연결 리스트 구현

```java
public class DoublyLinkedList {
    private DoublyNode head;
    private DoublyNode tail;
    private int size;

    public DoublyLinkedList() {
        this.head = null;
        this.tail = null;
        this.size = 0;
    }

    public void append(int data) {
        DoublyNode newNode = new DoublyNode(data);

        if (head == null) {
            head = tail = newNode;
        } else {
            tail.next = newNode;
            newNode.prev = tail;
            tail = newNode;
        }
        size++;
    }

    public void prepend(int data) {
        DoublyNode newNode = new DoublyNode(data);

        if (head == null) {
            head = tail = newNode;
        } else {
            newNode.next = head;
            head.prev = newNode;
            head = newNode;
        }
        size++;
    }
}
```

## ⏱️ 시간 복잡도

| 연산        | 배열 | 연결 리스트 |
| ----------- | ---- | ----------- |
| 접근        | O(1) | O(n)        |
| 검색        | O(n) | O(n)        |
| 삽입 (시작) | O(n) | O(1)        |
| 삽입 (끝)   | O(1) | O(n)        |
| 삭제 (시작) | O(n) | O(1)        |
| 삭제 (끝)   | O(1) | O(n)        |

## 📚 추가 학습 자료

### 온라인 자료

- [GeeksforGeeks Linked List](https://www.geeksforgeeks.org/data-structures/linked-list/)
- [LeetCode Linked List Problems](https://leetcode.com/tag/linked-list/)
- [Oracle Java Collections Framework](https://docs.oracle.com/javase/tutorial/collections/)

### 추천 도서

- "Introduction to Algorithms" - Thomas H. Cormen
- "Data Structures and Algorithms in Java" - Robert Lafore
- "Effective Java" - Joshua Bloch

## ✅ 체크리스트

- [ ] 연결 리스트의 기본 개념 이해
- [ ] 단일 연결 리스트 구현
- [ ] 이중 연결 리스트 구현
- [ ] 연결 리스트 뒤집기 알고리즘 구현
- [ ] 사이클 감지 알고리즘 구현
- [ ] 실습 문제 해결
- [ ] 시간 복잡도 분석

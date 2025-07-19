# 스택과 큐 (Stacks and Queues)

## 📚 개요

스택과 큐는 선형 자료구조로, 데이터의 삽입과 삭제가 특정 순서로 이루어집니다.

## 🎯 학습 목표

- 스택과 큐의 기본 개념 이해
- LIFO와 FIFO 원리 학습
- 스택과 큐를 활용한 알고리즘 구현

## 📖 주요 내용

### 스택 (Stack) - LIFO (Last In, First Out)

- **특징**: 마지막에 들어온 데이터가 먼저 나감
- **주요 연산**: push, pop, peek, isEmpty
- **응용**: 함수 호출, 괄호 검사, 후위 표기법

### 큐 (Queue) - FIFO (First In, First Out)

- **특징**: 먼저 들어온 데이터가 먼저 나감
- **주요 연산**: enqueue, dequeue, peek, isEmpty
- **응용**: 작업 스케줄링, BFS, 버퍼링

## 💻 구현 예제

### 스택 구현

```java
import java.util.ArrayList;
import java.util.EmptyStackException;

public class Stack<T> {
    private ArrayList<T> items;

    public Stack() {
        this.items = new ArrayList<>();
    }

    // 요소 추가
    public void push(T element) {
        items.add(element);
    }

    // 요소 제거 및 반환
    public T pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return items.remove(items.size() - 1);
    }

    // 최상단 요소 확인
    public T peek() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }
        return items.get(items.size() - 1);
    }

    // 스택이 비어있는지 확인
    public boolean isEmpty() {
        return items.size() == 0;
    }

    // 스택 크기
    public int size() {
        return items.size();
    }

    // 스택 비우기
    public void clear() {
        items.clear();
    }
}
```

### 큐 구현

```java
import java.util.ArrayList;
import java.util.NoSuchElementException;

public class Queue<T> {
    private ArrayList<T> items;

    public Queue() {
        this.items = new ArrayList<>();
    }

    // 요소 추가 (뒤쪽에)
    public void enqueue(T element) {
        items.add(element);
    }

    // 요소 제거 및 반환 (앞쪽에서)
    public T dequeue() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        return items.remove(0);
    }

    // 첫 번째 요소 확인
    public T peek() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        return items.get(0);
    }

    // 큐가 비어있는지 확인
    public boolean isEmpty() {
        return items.size() == 0;
    }

    // 큐 크기
    public int size() {
        return items.size();
    }

    // 큐 비우기
    public void clear() {
        items.clear();
    }
}
```

## 🔍 주요 알고리즘

### 1. 괄호 검사 (스택 활용)

```java
import java.util.Stack;
import java.util.Map;
import java.util.HashMap;

public class ParenthesesChecker {
    public static boolean isValidParentheses(String s) {
        Stack<Character> stack = new Stack<>();
        Map<Character, Character> pairs = new HashMap<>();
        pairs.put(')', '(');
        pairs.put('}', '{');
        pairs.put(']', '[');

        for (char c : s.toCharArray()) {
            if (c == '(' || c == '{' || c == '[') {
                stack.push(c);
            } else if (pairs.containsKey(c)) {
                if (stack.isEmpty() || stack.pop() != pairs.get(c)) {
                    return false;
                }
            }
        }

        return stack.isEmpty();
    }
}
```

### 2. 후위 표기법 계산

```java
import java.util.Stack;
import java.util.Map;
import java.util.HashMap;
import java.util.function.BinaryOperator;

public class PostfixEvaluator {
    public static int evaluatePostfix(String expression) {
        Stack<Integer> stack = new Stack<>();
        Map<String, BinaryOperator<Integer>> operators = new HashMap<>();
        operators.put("+", (a, b) -> a + b);
        operators.put("-", (a, b) -> a - b);
        operators.put("*", (a, b) -> a * b);
        operators.put("/", (a, b) -> a / b);

        String[] tokens = expression.split(" ");

        for (String token : tokens) {
            if (operators.containsKey(token)) {
                int b = stack.pop();
                int a = stack.pop();
                stack.push(operators.get(token).apply(a, b));
            } else {
                stack.push(Integer.parseInt(token));
            }
        }

        return stack.pop();
    }
}
```

### 3. 원형 큐 구현

```java
public class CircularQueue {
    private int[] items;
    private int front;
    private int rear;
    private int size;
    private int capacity;

    public CircularQueue(int k) {
        this.capacity = k;
        this.items = new int[k];
        this.front = -1;
        this.rear = -1;
        this.size = 0;
    }

    public boolean enqueue(int value) {
        if (isFull()) {
            return false;
        }

        if (isEmpty()) {
            front = 0;
        }

        rear = (rear + 1) % capacity;
        items[rear] = value;
        size++;
        return true;
    }

    public boolean dequeue() {
        if (isEmpty()) {
            return false;
        }

        if (front == rear) {
            front = -1;
            rear = -1;
        } else {
            front = (front + 1) % capacity;
        }
        size--;
        return true;
    }

    public int peek() {
        if (isEmpty()) {
            return -1;
        }
        return items[front];
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public boolean isFull() {
        return size == capacity;
    }
}
```

## 📝 연습 문제

### 스택 관련 문제

1. 괄호 검사
2. 후위 표기법 계산
3. 중위 표기법을 후위 표기법으로 변환
4. 스택을 이용한 큐 구현
5. 큐를 이용한 스택 구현

### 큐 관련 문제

1. BFS 구현
2. 원형 큐 구현
3. 우선순위 큐 구현
4. 슬라이딩 윈도우 최대값

## 🛠️ 구현 팁

### 연결 리스트를 이용한 스택

```java
class StackNode {
    int data;
    StackNode next;

    public StackNode(int data) {
        this.data = data;
        this.next = null;
    }
}

public class LinkedListStack {
    private StackNode top;
    private int size;

    public LinkedListStack() {
        this.top = null;
        this.size = 0;
    }

    public void push(int data) {
        StackNode newNode = new StackNode(data);
        newNode.next = top;
        top = newNode;
        size++;
    }

    public int pop() {
        if (isEmpty()) {
            throw new EmptyStackException();
        }

        int data = top.data;
        top = top.next;
        size--;
        return data;
    }

    public boolean isEmpty() {
        return top == null;
    }
}
```

### 우선순위 큐

```java
import java.util.ArrayList;
import java.util.Collections;

class QueueElement implements Comparable<QueueElement> {
    int element;
    int priority;

    public QueueElement(int element, int priority) {
        this.element = element;
        this.priority = priority;
    }

    @Override
    public int compareTo(QueueElement other) {
        return Integer.compare(this.priority, other.priority);
    }
}

public class PriorityQueue {
    private ArrayList<QueueElement> items;

    public PriorityQueue() {
        this.items = new ArrayList<>();
    }

    public void enqueue(int element, int priority) {
        QueueElement queueElement = new QueueElement(element, priority);
        items.add(queueElement);
        Collections.sort(items);
    }

    public int dequeue() {
        if (isEmpty()) {
            throw new NoSuchElementException("Queue is empty");
        }
        return items.remove(0).element;
    }

    public boolean isEmpty() {
        return items.isEmpty();
    }
}
```

### Java 내장 컬렉션 활용

```java
import java.util.Stack;
import java.util.Queue;
import java.util.LinkedList;
import java.util.PriorityQueue;

public class BuiltInCollections {
    public static void main(String[] args) {
        // Java 내장 Stack 사용
        Stack<Integer> stack = new Stack<>();
        stack.push(1);
        stack.push(2);
        int top = stack.pop(); // 2

        // Java 내장 Queue 사용 (LinkedList 구현)
        Queue<Integer> queue = new LinkedList<>();
        queue.offer(1);
        queue.offer(2);
        int front = queue.poll(); // 1

        // Java 내장 PriorityQueue 사용
        PriorityQueue<Integer> pq = new PriorityQueue<>();
        pq.offer(3);
        pq.offer(1);
        pq.offer(2);
        int min = pq.poll(); // 1 (최소값)
    }
}
```

## ⏱️ 시간 복잡도

| 연산 | 스택 | 큐   |
| ---- | ---- | ---- |
| 삽입 | O(1) | O(1) |
| 삭제 | O(1) | O(1) |
| 접근 | O(1) | O(1) |
| 검색 | O(n) | O(n) |

## 📚 추가 학습 자료

### 온라인 자료

- [GeeksforGeeks Stack](https://www.geeksforgeeks.org/stack-data-structure/)
- [GeeksforGeeks Queue](https://www.geeksforgeeks.org/queue-data-structure/)
- [LeetCode Stack Problems](https://leetcode.com/tag/stack/)
- [Oracle Java Collections Framework](https://docs.oracle.com/javase/tutorial/collections/)

### 추천 도서

- "Introduction to Algorithms" - Thomas H. Cormen
- "Data Structures and Algorithms in Java" - Robert Lafore
- "Effective Java" - Joshua Bloch

## ✅ 체크리스트

- [ ] 스택의 LIFO 원리 이해
- [ ] 큐의 FIFO 원리 이해
- [ ] 스택과 큐 구현
- [ ] 괄호 검사 알고리즘 구현
- [ ] 후위 표기법 계산 구현
- [ ] 원형 큐 구현
- [ ] 우선순위 큐 구현
- [ ] 실습 문제 해결

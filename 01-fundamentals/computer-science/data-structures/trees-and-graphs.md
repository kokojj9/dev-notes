# 트리와 그래프 (Trees and Graphs)

## 📚 개요

트리와 그래프는 계층적 구조와 관계를 표현하는 비선형 자료구조입니다.

## 🎯 학습 목표

- 트리와 그래프의 기본 개념 이해
- 이진 트리, BST, 힙 구현
- DFS, BFS 알고리즘 구현

## 📖 주요 내용

### 트리 (Tree)

- **특징**: 계층적 구조, 사이클 없음
- **종류**: 이진 트리, 이진 검색 트리, AVL 트리, 힙
- **응용**: 파일 시스템, DOM, 데이터베이스 인덱스

### 그래프 (Graph)

- **특징**: 노드와 엣지로 구성된 관계 표현
- **종류**: 무방향 그래프, 방향 그래프, 가중 그래프
- **응용**: 소셜 네트워크, 네비게이션, 네트워크

## 💻 구현 예제

### 이진 트리 노드

```java
class TreeNode {
    int val;
    TreeNode left;
    TreeNode right;

    public TreeNode(int val) {
        this.val = val;
        this.left = null;
        this.right = null;
    }
}
```

### 이진 검색 트리

```java
public class BinarySearchTree {
    private TreeNode root;

    public BinarySearchTree() {
        this.root = null;
    }

    public void insert(int val) {
        root = insertHelper(root, val);
    }

    private TreeNode insertHelper(TreeNode root, int val) {
        if (root == null) {
            return new TreeNode(val);
        }

        if (val < root.val) {
            root.left = insertHelper(root.left, val);
        } else if (val > root.val) {
            root.right = insertHelper(root.right, val);
        }

        return root;
    }

    public boolean find(int val) {
        return findHelper(root, val);
    }

    private boolean findHelper(TreeNode root, int val) {
        if (root == null) {
            return false;
        }

        if (val == root.val) {
            return true;
        } else if (val < root.val) {
            return findHelper(root.left, val);
        } else {
            return findHelper(root.right, val);
        }
    }

    public void delete(int val) {
        root = deleteHelper(root, val);
    }

    private TreeNode deleteHelper(TreeNode root, int val) {
        if (root == null) {
            return null;
        }

        if (val < root.val) {
            root.left = deleteHelper(root.left, val);
        } else if (val > root.val) {
            root.right = deleteHelper(root.right, val);
        } else {
            // 삭제할 노드를 찾은 경우
            if (root.left == null) {
                return root.right;
            } else if (root.right == null) {
                return root.left;
            }

            // 두 자식이 모두 있는 경우
            TreeNode minNode = findMin(root.right);
            root.val = minNode.val;
            root.right = deleteHelper(root.right, minNode.val);
        }

        return root;
    }

    private TreeNode findMin(TreeNode root) {
        while (root.left != null) {
            root = root.left;
        }
        return root;
    }
}
```

### 그래프 (인접 리스트)

```java
import java.util.*;

public class Graph {
    private Map<Integer, List<Integer>> adjacencyList;

    public Graph() {
        this.adjacencyList = new HashMap<>();
    }

    public void addVertex(int vertex) {
        adjacencyList.putIfAbsent(vertex, new ArrayList<>());
    }

    public void addEdge(int vertex1, int vertex2) {
        adjacencyList.get(vertex1).add(vertex2);
        adjacencyList.get(vertex2).add(vertex1);
    }

    public void removeEdge(int vertex1, int vertex2) {
        List<Integer> list1 = adjacencyList.get(vertex1);
        List<Integer> list2 = adjacencyList.get(vertex2);

        if (list1 != null) {
            list1.removeIf(v -> v == vertex2);
        }
        if (list2 != null) {
            list2.removeIf(v -> v == vertex1);
        }
    }

    public void removeVertex(int vertex) {
        List<Integer> neighbors = adjacencyList.get(vertex);
        if (neighbors != null) {
            for (int neighbor : new ArrayList<>(neighbors)) {
                removeEdge(vertex, neighbor);
            }
        }
        adjacencyList.remove(vertex);
    }

    public List<Integer> getNeighbors(int vertex) {
        return adjacencyList.getOrDefault(vertex, new ArrayList<>());
    }

    public void printGraph() {
        for (Map.Entry<Integer, List<Integer>> entry : adjacencyList.entrySet()) {
            System.out.println(entry.getKey() + " -> " + entry.getValue());
        }
    }
}
```

## 🔍 주요 알고리즘

### 1. 트리 순회 (Tree Traversal)

```java
import java.util.*;

public class TreeTraversal {
    // 전위 순회 (Pre-order): Root -> Left -> Right
    public static List<Integer> preorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        preorderHelper(root, result);
        return result;
    }

    private static void preorderHelper(TreeNode node, List<Integer> result) {
        if (node == null) return;

        result.add(node.val);
        preorderHelper(node.left, result);
        preorderHelper(node.right, result);
    }

    // 중위 순회 (In-order): Left -> Root -> Right
    public static List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        inorderHelper(root, result);
        return result;
    }

    private static void inorderHelper(TreeNode node, List<Integer> result) {
        if (node == null) return;

        inorderHelper(node.left, result);
        result.add(node.val);
        inorderHelper(node.right, result);
    }

    // 후위 순회 (Post-order): Left -> Right -> Root
    public static List<Integer> postorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        postorderHelper(root, result);
        return result;
    }

    private static void postorderHelper(TreeNode node, List<Integer> result) {
        if (node == null) return;

        postorderHelper(node.left, result);
        postorderHelper(node.right, result);
        result.add(node.val);
    }

    // 레벨 순회 (Level-order)
    public static List<Integer> levelorderTraversal(TreeNode root) {
        List<Integer> result = new ArrayList<>();
        if (root == null) return result;

        Queue<TreeNode> queue = new LinkedList<>();
        queue.offer(root);

        while (!queue.isEmpty()) {
            TreeNode current = queue.poll();
            result.add(current.val);

            if (current.left != null) {
                queue.offer(current.left);
            }
            if (current.right != null) {
                queue.offer(current.right);
            }
        }

        return result;
    }
}
```

### 2. 깊이 우선 탐색 (DFS)

```java
import java.util.*;

public class GraphDFS {
    public static List<Integer> dfs(Graph graph, int start) {
        List<Integer> result = new ArrayList<>();
        Set<Integer> visited = new HashSet<>();
        Stack<Integer> stack = new Stack<>();

        stack.push(start);
        visited.add(start);

        while (!stack.isEmpty()) {
            int current = stack.pop();
            result.add(current);

            List<Integer> neighbors = graph.getNeighbors(current);
            for (int neighbor : neighbors) {
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    stack.push(neighbor);
                }
            }
        }

        return result;
    }

    // 재귀적 DFS
    public static List<Integer> dfsRecursive(Graph graph, int start) {
        List<Integer> result = new ArrayList<>();
        Set<Integer> visited = new HashSet<>();
        dfsHelper(graph, start, visited, result);
        return result;
    }

    private static void dfsHelper(Graph graph, int vertex, Set<Integer> visited, List<Integer> result) {
        visited.add(vertex);
        result.add(vertex);

        for (int neighbor : graph.getNeighbors(vertex)) {
            if (!visited.contains(neighbor)) {
                dfsHelper(graph, neighbor, visited, result);
            }
        }
    }
}
```

### 3. 너비 우선 탐색 (BFS)

```java
import java.util.*;

public class GraphBFS {
    public static List<Integer> bfs(Graph graph, int start) {
        List<Integer> result = new ArrayList<>();
        Set<Integer> visited = new HashSet<>();
        Queue<Integer> queue = new LinkedList<>();

        queue.offer(start);
        visited.add(start);

        while (!queue.isEmpty()) {
            int current = queue.poll();
            result.add(current);

            for (int neighbor : graph.getNeighbors(current)) {
                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    queue.offer(neighbor);
                }
            }
        }

        return result;
    }

    // 최단 경로 찾기
    public static int shortestPath(Graph graph, int start, int end) {
        if (start == end) return 0;

        Set<Integer> visited = new HashSet<>();
        Queue<int[]> queue = new LinkedList<>();

        queue.offer(new int[]{start, 0});
        visited.add(start);

        while (!queue.isEmpty()) {
            int[] current = queue.poll();
            int vertex = current[0];
            int distance = current[1];

            for (int neighbor : graph.getNeighbors(vertex)) {
                if (neighbor == end) {
                    return distance + 1;
                }

                if (!visited.contains(neighbor)) {
                    visited.add(neighbor);
                    queue.offer(new int[]{neighbor, distance + 1});
                }
            }
        }

        return -1; // 경로가 없음
    }
}
```

## 📝 연습 문제

### 트리 관련 문제

1. 이진 트리의 최대 깊이
2. 이진 트리 레벨 순회
3. 이진 검색 트리 검증
4. 이진 트리에서 경로 합
5. 이진 트리 직렬화/역직렬화

### 그래프 관련 문제

1. 연결 요소 개수
2. 사이클 감지
3. 위상 정렬
4. 최단 경로 찾기
5. 최소 신장 트리

## 🛠️ 구현 팁

### 힙 (Heap) 구현

```java
import java.util.ArrayList;
import java.util.Collections;

public class MinHeap {
    private ArrayList<Integer> heap;

    public MinHeap() {
        this.heap = new ArrayList<>();
    }

    public void insert(int value) {
        heap.add(value);
        bubbleUp(heap.size() - 1);
    }

    private void bubbleUp(int index) {
        while (index > 0) {
            int parentIndex = (index - 1) / 2;

            if (heap.get(index) >= heap.get(parentIndex)) {
                break;
            }

            Collections.swap(heap, index, parentIndex);
            index = parentIndex;
        }
    }

    public int extractMin() {
        if (heap.isEmpty()) {
            throw new IllegalStateException("Heap is empty");
        }

        int min = heap.get(0);
        int lastElement = heap.remove(heap.size() - 1);

        if (!heap.isEmpty()) {
            heap.set(0, lastElement);
            bubbleDown(0);
        }

        return min;
    }

    private void bubbleDown(int index) {
        while (true) {
            int smallest = index;
            int leftChild = 2 * index + 1;
            int rightChild = 2 * index + 2;

            if (leftChild < heap.size() && heap.get(leftChild) < heap.get(smallest)) {
                smallest = leftChild;
            }

            if (rightChild < heap.size() && heap.get(rightChild) < heap.get(smallest)) {
                smallest = rightChild;
            }

            if (smallest == index) {
                break;
            }

            Collections.swap(heap, index, smallest);
            index = smallest;
        }
    }

    public int peek() {
        if (heap.isEmpty()) {
            throw new IllegalStateException("Heap is empty");
        }
        return heap.get(0);
    }

    public boolean isEmpty() {
        return heap.isEmpty();
    }

    public int size() {
        return heap.size();
    }
}
```

### Java 내장 컬렉션 활용

```java
import java.util.*;

public class TreeGraphCollections {
    public static void main(String[] args) {
        // Java 내장 PriorityQueue (힙)
        PriorityQueue<Integer> minHeap = new PriorityQueue<>();
        PriorityQueue<Integer> maxHeap = new PriorityQueue<>(Collections.reverseOrder());

        // TreeSet (이진 검색 트리 기반)
        TreeSet<Integer> treeSet = new TreeSet<>();
        treeSet.add(3);
        treeSet.add(1);
        treeSet.add(2);
        // 자동으로 정렬됨: [1, 2, 3]

        // TreeMap (이진 검색 트리 기반)
        TreeMap<String, Integer> treeMap = new TreeMap<>();
        treeMap.put("apple", 1);
        treeMap.put("banana", 2);
        // 키 기준으로 자동 정렬

        // 그래프 표현용 컬렉션
        Map<Integer, List<Integer>> adjacencyList = new HashMap<>();

        // 그래프 알고리즘용 자료구조
        Set<Integer> visited = new HashSet<>();
        Queue<Integer> bfsQueue = new LinkedList<>();
        Stack<Integer> dfsStack = new Stack<>();
    }
}
```

## ⏱️ 시간 복잡도

| 연산   | 이진 검색 트리 | 힙       | 그래프 |
| ------ | -------------- | -------- | ------ |
| 검색   | O(log n)       | O(n)     | O(V+E) |
| 삽입   | O(log n)       | O(log n) | O(1)   |
| 삭제   | O(log n)       | O(log n) | O(V+E) |
| 최소값 | O(log n)       | O(1)     | -      |

## 📚 추가 학습 자료

### 온라인 자료

- [GeeksforGeeks Tree](https://www.geeksforgeeks.org/binary-tree-data-structure/)
- [GeeksforGeeks Graph](https://www.geeksforgeeks.org/graph-data-structure-and-algorithms/)
- [LeetCode Tree Problems](https://leetcode.com/tag/tree/)
- [Oracle Java Collections Framework](https://docs.oracle.com/javase/tutorial/collections/)

### 추천 도서

- "Introduction to Algorithms" - Thomas H. Cormen
- "Data Structures and Algorithms in Java" - Robert Lafore
- "Effective Java" - Joshua Bloch

## ✅ 체크리스트

- [ ] 트리의 기본 개념 이해
- [ ] 그래프의 기본 개념 이해
- [ ] 이진 검색 트리 구현
- [ ] 힙 구현
- [ ] DFS 알고리즘 구현
- [ ] BFS 알고리즘 구현
- [ ] 트리 순회 알고리즘 구현
- [ ] 실습 문제 해결

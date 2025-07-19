# 해시 테이블 (Hash Tables)

## 📚 개요

해시 테이블은 키-값 쌍을 저장하는 자료구조로, 평균적으로 O(1) 시간에 데이터에 접근할 수 있습니다.

## 🎯 학습 목표

- 해시 테이블의 기본 개념 이해
- 해시 함수의 원리 학습
- 충돌 해결 방법 구현

## 📖 주요 내용

### 해시 테이블의 특징

- **구조**: 키-값 쌍을 저장하는 배열 기반 자료구조
- **접근**: 해시 함수를 통해 키를 인덱스로 변환
- **시간 복잡도**: 평균 O(1), 최악 O(n)
- **공간 복잡도**: O(n)

### 해시 함수

- **목적**: 키를 고정된 크기의 정수로 변환
- **요구사항**: 일관성, 균등 분포, 효율성
- **충돌**: 서로 다른 키가 같은 해시값을 가질 수 있음

## 💻 구현 예제

### 기본 해시 테이블 (체이닝 방식)

```java
import java.util.*;

class HashEntry {
    String key;
    Integer value;

    public HashEntry(String key, Integer value) {
        this.key = key;
        this.value = value;
    }
}

public class HashTable {
    private LinkedList<HashEntry>[] table;
    private int capacity;
    private int size;

    @SuppressWarnings("unchecked")
    public HashTable(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        this.table = new LinkedList[capacity];

        for (int i = 0; i < capacity; i++) {
            table[i] = new LinkedList<>();
        }
    }

    public HashTable() {
        this(16); // 기본 크기
    }

    private int hash(String key) {
        int hash = 0;
        int prime = 31;

        for (int i = 0; i < Math.min(key.length(), 100); i++) {
            char c = key.charAt(i);
            hash = (hash * prime + c) % capacity;
        }

        return Math.abs(hash);
    }

    public void put(String key, Integer value) {
        int index = hash(key);
        LinkedList<HashEntry> bucket = table[index];

        // 기존 키가 있는지 확인
        for (HashEntry entry : bucket) {
            if (entry.key.equals(key)) {
                entry.value = value; // 값 업데이트
                return;
            }
        }

        // 새로운 키-값 쌍 추가
        bucket.add(new HashEntry(key, value));
        size++;

        // 로드 팩터가 너무 높으면 리사이징
        if ((double) size / capacity > 0.75) {
            resize();
        }
    }

    public Integer get(String key) {
        int index = hash(key);
        LinkedList<HashEntry> bucket = table[index];

        for (HashEntry entry : bucket) {
            if (entry.key.equals(key)) {
                return entry.value;
            }
        }

        return null;
    }

    public boolean remove(String key) {
        int index = hash(key);
        LinkedList<HashEntry> bucket = table[index];

        Iterator<HashEntry> iterator = bucket.iterator();
        while (iterator.hasNext()) {
            HashEntry entry = iterator.next();
            if (entry.key.equals(key)) {
                iterator.remove();
                size--;
                return true;
            }
        }

        return false;
    }

    public boolean containsKey(String key) {
        return get(key) != null;
    }

    public List<String> keys() {
        List<String> keys = new ArrayList<>();

        for (LinkedList<HashEntry> bucket : table) {
            for (HashEntry entry : bucket) {
                keys.add(entry.key);
            }
        }

        return keys;
    }

    public List<Integer> values() {
        List<Integer> values = new ArrayList<>();

        for (LinkedList<HashEntry> bucket : table) {
            for (HashEntry entry : bucket) {
                values.add(entry.value);
            }
        }

        return values;
    }

    @SuppressWarnings("unchecked")
    private void resize() {
        LinkedList<HashEntry>[] oldTable = table;
        capacity *= 2;
        size = 0;
        table = new LinkedList[capacity];

        for (int i = 0; i < capacity; i++) {
            table[i] = new LinkedList<>();
        }

        // 모든 항목을 새 테이블에 재삽입
        for (LinkedList<HashEntry> bucket : oldTable) {
            for (HashEntry entry : bucket) {
                put(entry.key, entry.value);
            }
        }
    }

    public int size() {
        return size;
    }

    public boolean isEmpty() {
        return size == 0;
    }

    public double getLoadFactor() {
        return (double) size / capacity;
    }
}
```

## 🔍 충돌 해결 방법

### 1. 체이닝 (Chaining)

```java
// 위의 구현에서 사용한 방법
// 같은 인덱스에 LinkedList로 여러 키-값 쌍 저장
```

### 2. 개방 주소법 (Open Addressing)

```java
public class HashTableOpenAddressing {
    private String[] keys;
    private Integer[] values;
    private boolean[] deleted;
    private int capacity;
    private int size;

    public HashTableOpenAddressing(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        this.keys = new String[capacity];
        this.values = new Integer[capacity];
        this.deleted = new boolean[capacity];
    }

    private int hash(String key) {
        int hash = 0;
        int prime = 31;

        for (int i = 0; i < key.length(); i++) {
            hash = (hash * prime + key.charAt(i)) % capacity;
        }

        return Math.abs(hash);
    }

    public void put(String key, Integer value) {
        int index = hash(key);

        // 선형 탐사
        while (keys[index] != null && !deleted[index] && !keys[index].equals(key)) {
            index = (index + 1) % capacity;
        }

        if (keys[index] == null || deleted[index]) {
            size++;
        }

        keys[index] = key;
        values[index] = value;
        deleted[index] = false;
    }

    public Integer get(String key) {
        int index = hash(key);

        while (keys[index] != null) {
            if (!deleted[index] && keys[index].equals(key)) {
                return values[index];
            }
            index = (index + 1) % capacity;
        }

        return null;
    }

    public boolean remove(String key) {
        int index = hash(key);

        while (keys[index] != null) {
            if (!deleted[index] && keys[index].equals(key)) {
                deleted[index] = true;
                size--;
                return true;
            }
            index = (index + 1) % capacity;
        }

        return false;
    }
}
```

### 3. 이중 해싱 (Double Hashing)

```java
public class DoubleHashingTable {
    private String[] keys;
    private Integer[] values;
    private int capacity;
    private int size;

    public DoubleHashingTable(int capacity) {
        this.capacity = capacity;
        this.size = 0;
        this.keys = new String[capacity];
        this.values = new Integer[capacity];
    }

    private int hash1(String key) {
        int hash = 0;
        for (int i = 0; i < key.length(); i++) {
            hash = (hash * 31 + key.charAt(i)) % capacity;
        }
        return Math.abs(hash);
    }

    private int hash2(String key) {
        int hash = 0;
        for (int i = 0; i < key.length(); i++) {
            hash = (hash * 37 + key.charAt(i)) % (capacity - 1);
        }
        return Math.abs(hash) + 1; // 0이 되지 않도록
    }

    public void put(String key, Integer value) {
        int index = hash1(key);
        int step = hash2(key);

        while (keys[index] != null && !keys[index].equals(key)) {
            index = (index + step) % capacity;
        }

        if (keys[index] == null) {
            size++;
        }

        keys[index] = key;
        values[index] = value;
    }
}
```

## 📝 연습 문제

### 쉬운 문제

1. 문자열에서 첫 번째 반복되지 않는 문자 찾기
2. 두 배열의 교집합 찾기
3. 배열에서 중복 요소 찾기

### 중간 문제

1. 두 수의 합 (Two Sum)
2. 문자열에서 가장 많이 나오는 문자 찾기
3. 배열에서 연속된 부분 배열의 합이 0인 경우 찾기

### 어려운 문제

1. LRU 캐시 구현
2. LFU 캐시 구현
3. 분산 해시 테이블 설계

## 🛠️ 구현 팁

### 좋은 해시 함수의 특징

```java
public class HashFunctions {
    // 간단한 해시 함수 (문자열)
    public static int simpleHash(String key, int size) {
        int hash = 0;
        for (int i = 0; i < key.length(); i++) {
            hash = (hash + key.charAt(i)) % size;
        }
        return hash;
    }

    // 더 나은 해시 함수 (소수 사용)
    public static int polynomialHash(String key, int size) {
        int hash = 0;
        int prime = 31;

        for (int i = 0; i < key.length(); i++) {
            hash = (hash * prime + key.charAt(i)) % size;
        }

        return Math.abs(hash);
    }

    // Java의 String.hashCode() 구현과 유사
    public static int javaStyleHash(String key) {
        int hash = 0;
        for (int i = 0; i < key.length(); i++) {
            hash = 31 * hash + key.charAt(i);
        }
        return hash;
    }
}
```

### Java 내장 HashMap 활용

```java
import java.util.*;

public class HashMapExamples {
    public static void main(String[] args) {
        // Java 내장 HashMap 사용
        Map<String, Integer> map = new HashMap<>();

        // 기본 연산
        map.put("apple", 1);
        map.put("banana", 2);
        map.put("cherry", 3);

        // 값 조회
        Integer value = map.get("apple"); // 1

        // 키 존재 확인
        boolean hasKey = map.containsKey("apple"); // true

        // 모든 키 순회
        for (String key : map.keySet()) {
            System.out.println(key + " -> " + map.get(key));
        }

        // 모든 엔트리 순회
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            System.out.println(entry.getKey() + " -> " + entry.getValue());
        }

        // LinkedHashMap (삽입 순서 유지)
        Map<String, Integer> linkedMap = new LinkedHashMap<>();

        // TreeMap (키 정렬)
        Map<String, Integer> treeMap = new TreeMap<>();

        // ConcurrentHashMap (스레드 안전)
        Map<String, Integer> concurrentMap = new ConcurrentHashMap<>();
    }
}
```

### 로드 팩터 관리

```java
public class LoadFactorExample {
    public static void calculateLoadFactor() {
        Map<String, Integer> map = new HashMap<>();

        // HashMap의 초기 용량과 로드 팩터 설정
        Map<String, Integer> customMap = new HashMap<>(32, 0.75f);

        // 로드 팩터 = 저장된 요소 수 / 버킷 크기
        // 일반적으로 0.75가 시간과 공간의 좋은 균형점

        System.out.println("HashMap 크기: " + map.size());
        // 내부 용량은 접근할 수 없지만,
        // 보통 16에서 시작해서 로드 팩터 초과시 2배씩 증가
    }
}
```

## ⏱️ 시간 복잡도

| 연산 | 평균 | 최악 |
| ---- | ---- | ---- |
| 삽입 | O(1) | O(n) |
| 검색 | O(1) | O(n) |
| 삭제 | O(1) | O(n) |

## 📚 추가 학습 자료

### 온라인 자료

- [GeeksforGeeks Hash Table](https://www.geeksforgeeks.org/hashing-data-structure/)
- [LeetCode Hash Table Problems](https://leetcode.com/tag/hash-table/)
- [Oracle Java HashMap Documentation](https://docs.oracle.com/javase/8/docs/api/java/util/HashMap.html)

### 추천 도서

- "Introduction to Algorithms" - Thomas H. Cormen
- "Data Structures and Algorithms in Java" - Robert Lafore
- "Effective Java" - Joshua Bloch

## ✅ 체크리스트

- [ ] 해시 테이블의 기본 개념 이해
- [ ] 해시 함수 구현
- [ ] 체이닝을 이용한 충돌 해결
- [ ] 개방 주소법을 이용한 충돌 해결
- [ ] 로드 팩터와 리사이징 구현
- [ ] 실습 문제 해결
- [ ] 시간 복잡도 분석

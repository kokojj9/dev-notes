# 배열과 문자열 (Arrays and Strings)

## 📚 개요

배열과 문자열은 프로그래밍에서 가장 기본적이고 중요한 자료구조입니다.

## 🎯 학습 목표

- 배열의 기본 개념과 사용법 이해
- 문자열 조작 기법 학습
- 배열과 문자열 관련 알고리즘 문제 해결

## 📖 주요 내용

### 배열 (Arrays)

- **정의**: 연속된 메모리 공간에 저장된 같은 타입의 데이터 집합
- **특징**: 인덱스로 접근 가능, 고정 크기 (일부 언어에서)
- **시간 복잡도**: 접근 O(1), 검색 O(n), 삽입/삭제 O(n)

### 문자열 (Strings)

- **정의**: 문자들의 시퀀스
- **특징**: 불변성 (immutable), 다양한 인코딩 지원
- **주요 연산**: 연결, 분할, 검색, 치환

## 💻 실습 예제

### 배열 기본 연산

```java
import java.util.*;

public class ArrayExample {
    public static void main(String[] args) {
        // 배열 생성
        int[] arr = {1, 2, 3, 4, 5};

        // 접근
        System.out.println(arr[0]); // 1

        // 순회
        for (int i = 0; i < arr.length; i++) {
            System.out.println(arr[i]);
        }

        // Enhanced for loop 사용
        for (int item : arr) {
            System.out.println(item);
        }

        // ArrayList 사용 (동적 배열)
        ArrayList<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);

        // Stream API 사용
        Arrays.stream(arr).forEach(System.out::println);
    }
}
```

### 문자열 기본 연산

```java
public class StringExample {
    public static void main(String[] args) {
        String str = "Hello World";

        // 길이
        System.out.println(str.length()); // 11

        // 접근
        System.out.println(str.charAt(0)); // "H"

        // 분할
        String[] words = str.split(" "); // ["Hello", "World"]

        // 연결
        String newStr = "Hello" + " " + "World";

        // StringBuilder 사용 (효율적인 문자열 조작)
        StringBuilder sb = new StringBuilder();
        sb.append("Hello").append(" ").append("World");
        String result = sb.toString();
    }
}
```

## 🔍 주요 알고리즘

### 1. 배열 순회

- **선형 순회**: O(n)
- **이진 검색**: O(log n) - 정렬된 배열에서만

### 2. 문자열 조작

- **문자열 뒤집기**
- **팰린드롬 확인**
- **문자열 압축**

### 3. 슬라이딩 윈도우

- **고정 크기 윈도우**
- **가변 크기 윈도우**

## 📝 연습 문제

### 쉬운 문제

1. 배열의 최대값 찾기
2. 문자열 뒤집기
3. 배열에서 특정 요소 찾기

### 중간 문제

1. 두 배열의 교집합 찾기
2. 문자열에서 가장 많이 나오는 문자 찾기
3. 배열에서 중복 요소 제거

### 어려운 문제

1. 배열에서 합이 특정 값이 되는 두 요소 찾기
2. 문자열에서 가장 긴 팰린드롬 찾기
3. 배열에서 연속된 부분 배열의 최대 합 찾기

## 🛠️ 구현 팁

### Java에서의 배열과 컬렉션

```java
import java.util.*;
import java.util.stream.Collectors;

public class ArrayTips {
    public static void main(String[] args) {
        int[] arr = {1, 2, 3, 4, 5};

        // Stream API를 이용한 배열 조작

        // map: 변환
        int[] doubled = Arrays.stream(arr)
                             .map(x -> x * 2)
                             .toArray();

        // filter: 필터링
        int[] evens = Arrays.stream(arr)
                           .filter(x -> x % 2 == 0)
                           .toArray();

        // reduce: 축약
        int sum = Arrays.stream(arr)
                       .reduce(0, Integer::sum);

        // List로 변환하여 더 많은 기능 사용
        List<Integer> list = Arrays.stream(arr)
                                  .boxed()
                                  .collect(Collectors.toList());
    }
}
```

### 문자열 처리

```java
import java.util.regex.Pattern;
import java.util.regex.Matcher;

public class StringProcessing {
    public static void main(String[] args) {
        String str = "Hello123World";

        // 정규식 활용
        Pattern pattern = Pattern.compile("\\d+");
        Matcher matcher = pattern.matcher(str);

        // 숫자만 추출
        while (matcher.find()) {
            System.out.println(matcher.group());
        }

        // 대소문자 변환
        String upper = str.toUpperCase();
        String lower = str.toLowerCase();

        // 문자열 검사
        boolean hasDigits = str.matches(".*\\d+.*");
    }
}
```

## 📚 추가 학습 자료

### 온라인 자료

- [Oracle Java Documentation - Arrays](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/arrays.html)
- [Oracle Java Documentation - String](https://docs.oracle.com/javase/8/docs/api/java/lang/String.html)
- [LeetCode Array Problems](https://leetcode.com/tag/array/)

### 추천 도서

- "Introduction to Algorithms" - Thomas H. Cormen
- "Effective Java" - Joshua Bloch
- "Java: The Complete Reference" - Herbert Schildt

## ✅ 체크리스트

- [ ] 배열의 기본 개념 이해
- [ ] 문자열 조작 기법 학습
- [ ] 배열과 문자열 관련 알고리즘 구현
- [ ] 실습 문제 해결
- [ ] 시간 복잡도 분석
- [ ] 최적화 기법 학습

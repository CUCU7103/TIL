
# Java 연산자(Operator) 정리

## 1. 기본 연산자

### 1) 대입 연산자 (=)
- 변수에 값을 저장할 때 사용

### 2) 산술 연산자

| 연산자 | 의미     |
| :----: | :------ |
|   +    | 더하기   |
|   -    | 빼기     |
|   *    | 곱하기   |
|   /    | 나누기   |
|   %    | 나머지   |




#### 예시: +, -
```java
int a = 5, b = 10;
System.out.println(a + b); // 15
System.out.println(b - a); // 5
```

#### 예시: *, /

```java
int a = 5, b = 10;
System.out.println(a * b); // 50
System.out.println(b / a); // 2
// int 타입끼리 나누면 소수점 이하 버림
// 실수 결과가 필요하면 double이나 float 사용 필요
```

#### 예시: %

```java
int a = 53, b = 10;
System.out.println(a % b); // 3
```

---

## 2. 복합 대입 연산자

- 산술 연산자와 대입 연산자를 결합한 형태

| 연산자 | 예시   | 의미      |
| :----: | :----- | :-------- |
|   +=   | x += 5 | x = x + 5 |
|   -=   | x -= 5 | x = x - 5 |
|   *=   | x *= 5 | x = x * 5 |
|   /=   | x /= 5 | x = x / 5 |
|   %=   | x %= 5 | x = x % 5 |

#### 예시 코드

```java
int value = 10;
value += 5;  // 15
value -= 5;  // 10
value *= 5;  // 50
value /= 5;  // 10
value %= 5;  // 0
```

---

## 3. 단항 연산자

### 1) 부호 연산자

- `+`, `-` : 값의 부호를 바꿉니다.

### 2) 증감 연산자 (`++`, `--`)

- 변수의 값을 1씩 증가/감소

```java
int v = 1;
System.out.println(v++); // 1 (출력 후 1 증가, v=2)
System.out.println(v);   // 2
System.out.println(++v); // 3 (증가 후 출력)
```

### 3) 부정 연산자 (`!`)

- boolean 값을 반대로 만듦

```java
boolean flag = true;
System.out.println(!flag); // false
```

---

## 4. 연산자 우선순위

1. 단항 연산자 (`++`, `--`, `+`, `-`, `!`, `~`)
2. 산술 연산자 (`*`, `/`, `%`)
3. 산술 연산자 (`+`, `-`)

---

## 5. 비교 연산자

| 연산자 | 의미        | 사용 가능 타입 |
| :----: | :---------- | :------------- |
|   ==   | 같다        | 모든 타입      |
|   !=   | 같지 않다   | 모든 타입      |
|   >    | 크다        | 숫자           |
|   <    | 작다        | 숫자           |
|   >=   | 크거나 같다 | 숫자           |
|   <=   | 작거나 같다 | 숫자           |

- 결과는 항상 boolean (true/false)

---

## 6. 논리 연산자

| 연산자 | 의미     | 예시                   |
| :----: | :------- | :--------------------- |
|   &&   | 논리 AND | (a > 0) && (b < 5)     |
|  \|\|  | 논리 OR  | (a == 1) \|\| (b == 2) |
|   !    | 논리 NOT | !(a == b)              |

---

## 7. 삼항 연산자

- `변수 = (조건식) ? 참일 때 값 : 거짓일 때 값;`

```java
int score = 80;
String pass = (score >= 60) ? "합격" : "불합격";
System.out.println(pass); // "합격"
```

---

## 예제: 조건에 따른 처리

```java
public class OperatorConditionalTriple {
    public static void main(String[] args) {
        doBlindDate(30);
        doBlindDate(80);
    }
    public static void doBlindDate(int point) {
        boolean doBlindDateFlag = (point >= 80) ? true : false;
        System.out.println(point + " : " + doBlindDateFlag);
    }
}
// 결과
// 30 : false
// 80 : true
```

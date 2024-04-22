# 문제

1. 우리는 입력값과 연산 수행 시간의 상관관계를 나타내는 척도를 시간 복잡도라고 한다. (o/x)

2. 해당 코드의 시간복잡도는 O(n^3)이다.(o/x)
```java
for(int i=0; i<n; i++) {

    for(int j=0; j<n; j++) {

        for(int k=0; k<n; k++) {

            System.out.println(n * j + k);

        }

    }

}
```

3. 해당 코드의 시간복잡도는 O(n^2)이다. (o/x)
```java
for(int i=0; i<n; i++) {

    for(int j=0; j<m; j++) {

        System.out.println(n * m);

    }

}
```

4. 해당 코드의 시간복잡도는 O(n^2)이다. (o/x)
```java
public static int pibo(int n) {

    if (n == 0) return 0;
    else if (n == 1) return 1;
    return pibo(n - 1) + pibo(n - 2);

}
```

5. 해당 코드의 시간복잡도는 O(n^2)이다. (o/x)
```java

for(int i=0; i<n; i++) {
    System.out.println(n);

}

for(int i=0; i<n; i++) {
    for(int j=0; j<n; j++){
        System.out.println(n);
    {
}
```

---

#### 답

1. 우리는 입력값과 연산 수행 시간의 상관관계를 나타내는 척도를 시간 복잡도라고 한다. 답 O
[](https://github.com/witwint/TIL/assets/108222981/7a2bb6e2-0515-4249-b021-49fa2c8f7d45)

2. 해당 코드의 시간복잡도는 O(n^3)이다. 답 O
```java
for(int i=0; i<n; i++) {

    for(int j=0; j<n; j++) {

        for(int k=0; k<n; k++) {

            System.out.println(n * j + k);

        }

    }

}
```
반복문이 3번 겹치기 때문에 시간복잡도가 n^3이 나온다.

3. 해당 코드의 시간복잡도는 O(n^2)이다. 답 X
```java
for(int i=0; i<n; i++) {

    for(int j=0; j<m; j++) {

        System.out.println(n * m);

    }

}
```
이중 포문 같지만 엄연히 다르다. n번 반복하는 반복문을 두 번 중첩시킨 것이 아니라 n번 반복문과 m번 반복문을 중첩시켰다. 막말로 n이 999999999999999인데 m은 1일 수도 있기 때문에 같은 n을 for에서 쓰는것과 달리 O(nm)이라고 나타내 주어야 한다.

4. 해당 코드의 시간복잡도는 O(n^2)이다. 답 X
```java
public static int pibo(int n) {

    if (n == 0) return 0;
    else if (n == 1) return 1;
    return pibo(n - 1) + pibo(n - 2);

}
```
위 코드는 피보나치수열을 구현한 것이다. pibo(n)을 호출하면 pibo(n-1)과 pibo(n-2)가 호출된다. 그리고 이 호출 작업은 총 n번 반복되기 때문에 이를 Big-O 표기법으로 나타내면 O(2ⁿ)이 된다.  
![](https://github.com/witwint/TIL/assets/108222981/dc066e43-38b1-4f96-82aa-4ace31932b02)  


5. 해당 코드의 시간복잡도는 O(n^2)이다. 답O
```java
for(int i=0; i<n; i++) {
    System.out.println(n);

}

for(int i=0; i<n; i++) {
    for(int j=0; j<n; j++){
        System.out.println(n);
    {
}
```
위 코드는 O(n²)와 O(n)이 합쳐져 O(n²+n)이 될 것 같지만 유감스럽게도 O(n²)이 된다. n²에 비해서 n은 영향력이 낮으므로 이를 무시하고 표기하는 것이 규칙이다. 앞서 상수항과 영향력이 낮은 항을 무시하는 이유는 Big-O 표기법이 실제 소요시간을 나타내기 위한 방법이 아니라 자료의 수가 증가함에 따라 소요시간이 얼마나 증가하는지를 나타내기 위한 방법이기 때문이다.  

O(?) <- 이 괄호 안에 들어가는 식을 계산해서 1억당 1초 정도로 생각할 수 있다고 한다. 예를 들어 n이 1억인데 시간 복잡도가 O(n)이면 소요시간이 1초정도 걸린다는 뜻이다. n이 1억, m이 10, 시간 복잡도가 O(nm)이면 소요시간이 10초가 된다는 것이다.


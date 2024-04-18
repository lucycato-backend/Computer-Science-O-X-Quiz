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
if(n%2 == 0) {
    System.out.println("짝수");
} else {
    System.out.println("홀수");
}

for(int i=0; i<n; i++) {
    System.out.println(n);

}
```

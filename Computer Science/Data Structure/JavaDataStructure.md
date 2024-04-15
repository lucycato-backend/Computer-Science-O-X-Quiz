
* 이진탐색 트리(BST)의 평균 시간 복잡도는 O(logn)이며 최악의 시간 복잡도는 O(n)이다

* 자바에서 HashSet이 HashMap보다 성능이 좋다.

* 자바 ArrayList의 contains 시간복잡도는 O(n)이다.

* 자바에서 HashMap과 달리 HashTable의 특징은 Thread-safe한것이다.

* 자바의 ConcurrentHashMap은 검색 속도 향상을 위해 나온 클래스 이다.

* 자바에서 map을 사용할 때 입력 순서를 보장할 수 있는 방법은 없다.

* 레드 블랙트리는 자기 균형 이진 탐색 트리이다.

* 자바의 HashMap은 해쉬 충돌이 생겼을때 개방 주소법(Open Addressing)을 사용한다.

* 자바의 HashMap버킷 크기는 고정이다.

* 자바의 HashMap은 해시 버킷 체이닝에 LinkedList만 사용한다.


---

* 이진탐색 트리(BST)의 평균 시간 복잡도는 O(logn)이며 최악의 시간 복잡도는 O(n)이다  
답 o
![1](https://github.com/witwint/TIL/assets/108222981/2900d9be-7dee-4130-afcc-1767f537368d)
![1](https://github.com/witwint/TIL/assets/108222981/40a3d58b-b174-4cd3-9a41-adc4feb3427d)
자주쓰는 시간 복잡도 평균과 최악의 시간 복잡도를 고려하여 사용해야 합니다.

---

* 자바에서 HashSet이 HashMap보다 성능이 좋다.  
답 x
![1](https://github.com/witwint/TIL/assets/108222981/6f39c929-c2ee-4f89-a7e9-87ffcd280544)
HashSet 내부에서 HashMap을 선언하여 사용하기에 차이가 없다.

---

* 자바 ArrayList의 contains 시간복잡도는 O(n)이다.  
답o
![1](https://github.com/witwint/TIL/assets/108222981/6e2e9356-f358-4382-a8c9-91ad9c54d171)
![1](https://github.com/witwint/TIL/assets/108222981/7d1d42e9-0b35-4613-a511-bb71eef8680e)
실제로 그냥 인덱스를 돌려서 값이 같은걸 찾고 리턴해준다.

---

* 자바에서 HashMap과 달리 HashTable의 특징은 Thread-safe한것이다.  
답 o
```java
public class Hashtable<K,V>
    extends Dictionary<K,V>
    implements Map<K,V>, Cloneable, java.io.Serializable {

    public synchronized int size() { }

    @SuppressWarnings("unchecked")
    public synchronized V get(Object key) { }

    public synchronized V put(K key, V value) { }
}
```
클래스를 확인해보면 `synchronized`통해 Thread-safe하게 사용할 수 있습니다. 다만 병목현상때문에 현제는 거의 사용하지 않습니다.  
자바에서 지원하느 Synchronized 키워드는 여러개의 스레드가 한개의 자원을 사용하고자 할 때, 현재 데이터를 사용하고 있는 해당 스레드를 제외하고 나머지 스레드들은 데이터에 접근 할 수 없도록 막는 개념입니다.  

---

* 자바의 ConcurrentHashMap은 검색 속도 향상을 위해 나온 클래스 이다.  
답 X
Hashtable 클래스의 단점을 보완하면서 Multi-Thread 환경에서 사용할 수 있도록 나온 클래스가 바로 ConcurrentHashMap 입니다.
```java
public class ConcurrentHashMap<K,V> extends AbstractMap<K,V>
    implements ConcurrentMap<K,V>, Serializable {

    public V get(Object key) {}

    public boolean containsKey(Object key) { }

    public V put(K key, V value) {
        return putVal(key, value, false);
    }

    final V putVal(K key, V value, boolean onlyIfAbsent) {
        if (key == null || value == null) throw new NullPointerException();
        int hash = spread(key.hashCode());
        int binCount = 0;
        for (Node<K,V>[] tab = table;;) {
            Node<K,V> f; int n, i, fh;
            if (tab == null || (n = tab.length) == 0)
                tab = initTable();
            else if ((f = tabAt(tab, i = (n - 1) & hash)) == null) {
                if (casTabAt(tab, i, null,
                             new Node<K,V>(hash, key, value, null)))
                    break;                   // no lock when adding to empty bin
            }
            else if ((fh = f.hash) == MOVED)
                tab = helpTransfer(tab, f);
            else {
                V oldVal = null;
                synchronized (f) {
                    if (tabAt(tab, i) ==
.
.
.
```
synchronized 키워드가 메소드 전체에 붙어 있지 않습니다. get() 메소드에는 아예 synchronized가 존재하지 않고, put() 메소드에는 중간에 synchronized 키워드가 존재하는 것을 볼 수 있습니다. 이것을 좀 더 정리해보면 ConcurrentHashMap은 읽기 작업에는 여러 쓰레드가 동시에 읽을 수 있지만, 쓰기 작업에는 특정 세그먼트 or 버킷에 대한 Lock을 사용한다는 것입니다.  
(https://velog.io/@kimunche/ConcurrentHashMap) 참고자료
![1](https://github.com/witwint/TIL/assets/108222981/29410bcc-f317-49b8-8506-ec865824aed3)
![2](https://github.com/witwint/TIL/assets/108222981/4041ba56-067e-4a05-9359-8377ca8d4378)

---

* 자바에서 map을 사용할 때 입력 순서를 보장할 수 있는 방법은 없다.  
답 x
`LinkedHashMap`이 있습니다. LinkedHashMap은 HashMap을 상속하여 만들어진 클래스이고 따라서 HashMap의 특징을 가지고 습니다. 다른 점이라고 한다면, Node 객체를 Entry 객체로 감싸서 저장된 키의 순서를 보존하고 있다는 점입니다. 동기화가 되어있지 않기 때문에 멀티스레드 환경에서 사용할 때는 주의가 필요합니다.

---

* 레드 블랙트리는 자기 균형 이진 탐색 트리이다.  
답 o
레드블랙트리는 최대힙 최소힙이런 트리가 정리를 하면서 데이터를 정리헤 저장하는거처럼 좌우 방향을 기준으로 자신의 왼쪽 서브 트리에는 현재 노드보다 값이 작은 것, 오른쪽 서브 트리에는 값이 큰 것들만 가질 수 있게 만든 트리입니다.
![`](https://github.com/witwint/TIL/assets/108222981/1db141a6-cff5-4493-aee2-6210108d4721)
자세한 내용 (https://jwdeveloper.tistory.com/280)

---

* 자바의 HashMap은 해쉬 충돌이 생겼을때 개방 주소법(Open Addressing)을 사용한다.  
답 x

---

* 자바의 HashMap버킷 크기는 고정이다.  
답 x

---

* 자바의 HashMap은 해시 버킷 체이닝에 LinkedList만 사용한다.  
답 x
자바의 HashMap은 체이닝 방법을 사용해서 해쉬 충돌을 해결한다.
특이점이 2가지 있는데 우선 버킷의 경우 해시 버킷 개수의 기본 값은 16이고, 데이터의 개수가 임계점에 이를 때마다 해시 버킷 개수의 크기를 두 배씩 증가시킨다. 그리고 임계점은 지금 버킷 개수의 3/4이다.
```java
	

//8개 이상의 키-값 쌍이 모이면 트리로 변경
static final int TREEIFY_THRESHOLD = 8;

//6개 이하로 떨어지면 링크드 리스트로 변경
static final int UNTREEIFY_THRESHOLD = 6;

```
해쉬 충돌 체이닝의 경우 버킷에 체이닝 된 데이터의 수에 따라 링크드 리스트와 트리를 변경해가며 사용한다. 트리는 레드블랙 트리를 사용하고 링크드 리스트와 트리를 변경하는 기준은 위와 같이 설정되어 있다.  



1. HashMap은 해싱함수를 통해 인덱스를 산출한다.

2. HashMap은 인덱스를 통한 접근으로 시간 복잡도 O(1)의 빠른 성능을 자랑한다.

3. key는 무한하지만 인덱스는 한정되어 있어 충돌은 불가피하다.

4. 충돌을 줄이기 위해 HashMap은 버킷의 사이즈를 조절한다.

5. 충돌이 일어날 시, 충돌 수가 적으면 LinkedList 방식으로 충돌된 객체들을 관리하다가, 임계점을 넘으면 Red-Black Tree 방식으로 객체들을 저장한다.

6. 시간 복잡도는 Linked List가 O(n), Red-Black Tree가 O(log n)이다.
![1](https://github.com/witwint/TIL/assets/108222981/6cc7d837-5a28-4dd4-b78a-754ddb6f88c1)

![1](https://github.com/witwint/TIL/assets/108222981/0dbc316f-e1c5-45d7-b473-703e7efa003b)

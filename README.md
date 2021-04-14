					Greedy2-11 : 김선민, 이예린, 정윤아
# 부분 배낭 문제
------

## 1. 배낭 문제란?

 **배낭(Knapsack) 문제**는 조합 최적화 문제의 일종으로 용량이 정해진 배낭과 무게와 가치가 다른 n개의 물건의 집합이 주어졌을 때 최대의 가치를 갖도록 배낭에 넣을 물건들의 부분집합을 구하는 문제이다. 

<img src="https://www.bing.com/images/blob?bcid=S310Y7YikaACTA" alt="배낭 문제" width="350" height="270" />



 이 배낭문제는 물건을 분할할 수 있는 경우와 물건을 분할할 수 없는 경우 두 가지로 나눌 수 있는데, 물건을 부분적으로 담는 것이 허용이 되는 경우의 배낭문제를 **부분 배낭 문제(Fractional Knapsack Problem)**, 물건을 분할 할 수 없고 통째로 넣어햐하는 경우의 배낭문제를 **0-1 배낭문제(0-1 Knapsack Problem)**라 부른다. 부분 배낭 문제는 [그리디 알고리즘](https://namu.wiki/w/%EA%B7%B8%EB%A6%AC%EB%94%94%20%EC%95%8C%EA%B3%A0%EB%A6%AC%EC%A6%98)으로 구현 가능하고, 0-1 배낭 문제는 그리디 알고리즘으로는 최적해를 찾을 수 없다. 따라서 동적 계획 알고리즘, 백트래킹 기법, 분기 한정 기법으로 해결해야 한다.

 이번에 구현해 볼 알고리즘은 부분 배낭 문제로 부분 배낭 문제에서는 물건을 부분적으로 배낭에 담을 수 있으므로, 최적해를 위해서 *욕심을 내어*  단위 무게 당 가장 가치가 큰 물건을 배낭에 넣고, 계속해서 그 다음으로 가치가 큰 물건을 넣는다. 그런데 만일 그 다음으로 가치가 큰 물건을 통째로 배낭에 넣을 수 없게 되면 배낭에 넣을 수 있을 만큼만 물건을 부분적으로 배낭에 담는다.


## 2. 부분 배낭 문제 그리디 알고리즘





## 3. 자바 코드

```java
import java.util.*;

public class Knapsack {
    static class Item {
        int w, v;         //물건의 무게, 가치
        String name;      //물건의 이름
        double cost;      //단위당 가치

        public Item(String n, int w, int v) {
            this.name = n;
            this.w = w;
            this.v = v;
            this.cost = (double) v / (double) w;
        }
    }

    public static Item[] SortItem(Item[] L) {     //단위당 가치를 내림차순 정렬
        for (int i = 0; i < L.length - 1; i++) {
            for (int j = i + 1; j < L.length; j++) {
                if (L[i].cost < L[j].cost) {
                    Item temp = L[i];
                    L[i] = L[j];
                    L[j] = temp;
                }
            }
        }
        return L;
    }

    static int Llen = 0;

    public static Item[] FractionalKnapsack(Item[] S, int C) {
        Item[] L = new Item[S.length];
        int w = 0, v = 0;                     //담긴 물건의 무게, 가치
        int x = 0;                            //단위당 가치가 가장 큰 물건의 인덱스

        while (w + S[x].w <= C) {                        //배낭이 가득찰 때까지
            L[Llen++] = new Item(S[x].name, S[x].w, S[x].v);
            w += S[x].w;
            v += S[x].v;                                 //무게와 가치 증가
            x++;
        }

        if (C - w > 0) {           //더 담을 수 있는 만큼 추가
            L[Llen++] = new Item(S[x].name, C - w, (int) (S[x].cost * (C - w)));
            v += (int) (S[x].cost * (C - w));
            w += C - w;
            x++;
        }
        System.out.printf("배낭에 담긴 총 가치는 %d이고,\n", v);

        return L;
    }

    public static void main(String[] args) {
        Scanner sc = new Scanner(System.in);

        System.out.println("물건의 개수와 배낭의 용량을 입력해주세요.");
        int n = sc.nextInt();          //물건의 개수
        int c = sc.nextInt();          //배낭의 용량

        Item[] L = new Item[n];

        System.out.println("각 물건의 이름, 무게, 가치를 입력해주세요.");
        for (int i = 0; i < n; i++) {
            String name = sc.next();
            int w = sc.nextInt();
            int v = sc.nextInt();           //무게, 가치
            L[i] = new Item(name, w, v);
        }

        L = FractionalKnapsack(SortItem(L), c);

        for (int i = 0; i < Llen; i++) {
            System.out.println(L[i].name + " : " + L[i].w);
        }
        System.out.print("가 담겨있다.");

        sc.close();
    }
}
```



## 4.  코드 결과





## 5.  성능 평가

이 알고리즘은 이중 for문을 이용 했기 때문에 시간 복잡도는 O(n^2)이며 이를 그래프로 나타내면 이와 같다.
![n^2](https://user-images.githubusercontent.com/81538527/114697087-6f01bd80-9d58-11eb-9309-f4ee90e55381.png)

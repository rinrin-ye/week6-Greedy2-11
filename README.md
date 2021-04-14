								Greedy2-11 : 김선민, 이예린, 정윤아
# 부분 배낭 문제

------

## 1. 배낭 문제란?





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
![n^2](https://github.com/kssssm/FractionalKanpsack/blob/master/스크린샷%202021-04-14%20오후%207.20.12.png)

        

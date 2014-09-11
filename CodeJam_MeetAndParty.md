# Meet and Party

[원문제](https://code.google.com/codejam/contest/2929486/dashboard#s=p1)는 영어이다.

## 풀이... 

```java
package codeJam.google.com;  
  
import java.io.File;  
import java.io.FileWriter;  
import java.io.IOException;  
import java.util.ArrayList;  
import java.util.Collections;  
import java.util.Scanner;  
  
/** 
 * @author Zhenyi 2013 Dec 26, 2013 2:17:53 PM 
 */  
class Pair implements Comparable<Pair> {  
    Integer first;  
    Integer second;  
  
    @Override  
    public int compareTo(Pair o) {  
        int compare = this.first.compareTo(o.first);  
        return compare != 0 ? compare : this.second.compareTo(o.second);  
    }  
  
    public Pair(int x, int y) {  
        this.first = x;  
        this.second = y;  
    }  
  
}  
  
public class MeetAndParty {  
    public static void main(String[] args) throws IOException {  
        Scanner in = new Scanner(new File(  
                "C:/Users/Zhenyi/Downloads/B-small-practice.in"));  
        FileWriter out = new FileWriter(  
                "C:/Users/Zhenyi/Downloads/B-small-practice.out");  
        // Scanner in = new Scanner(new  
        // File("C:/Users/Zhenyi/Downloads/B-large-practice.in"));  
        // FileWriter out = new  
        // FileWriter("C:/Users/Zhenyi/Downloads/B-large-practice.out");  
  
        int T = in.nextInt();  
  
        for (int cases = 1; cases <= T; cases++) {  
            int B = in.nextInt();  
            ArrayList<Integer> x = new ArrayList<Integer>();  
            ArrayList<Integer> y = new ArrayList<Integer>();  
            ArrayList<Pair> pair = new ArrayList<Pair>();  
            for (int i = 0; i < B; i++) {  
                int x1 = in.nextInt();  
                int y1 = in.nextInt();  
                int x2 = in.nextInt();  
                int y2 = in.nextInt();  
                for (int j = x1; j <= x2; j++) {  
                    for (int k = y1; k <= y2; k++) {  
                        x.add(j);  
                        y.add(k);  
                        pair.add(new Pair(j, k));  
                    }  
                }  
            }  
            Collections.sort(x);  
            Collections.sort(y);  
            Collections.sort(pair);  
  
            ArrayList<Long> sumx = new ArrayList<Long>();  
            ArrayList<Long> sumy = new ArrayList<Long>();  
            sumx.add((long) 0);  
            sumy.add((long) 0);  
            for (int i = 0; i < x.size(); i++) {  
                Long tmp = sumx.get(sumx.size() - 1);  
                sumx.add(tmp + x.get(i));  
            }  
            for (int i = 0; i < y.size(); i++) {  
                Long tmp = sumy.get(sumy.size() - 1);  
                sumy.add(tmp + y.get(i));  
            }  
            long bestCost = Long.MAX_VALUE;  
            int resultx = 0;  
            int resulty = 0;  
  
            for (int i = 0; i < pair.size(); i++) {  
                int firstx = Collections.binarySearch(x, pair.get(i).first);  
                int firsty = Collections.binarySearch(y, pair.get(i).second);  
                long cost = (firstx + 1) * (long) pair.get(i).first  
                        - sumx.get(firstx + 1) + sumx.get(sumx.size() - 1)  
                        - sumx.get(firstx + 1) - (pair.size() - firstx - 1)  
                        * (long) pair.get(i).first + (firsty + 1)  
                        * (long) pair.get(i).second - sumy.get(firsty + 1)  
                        + sumy.get(sumy.size() - 1) - sumy.get(firsty + 1)  
                        - (pair.size() - firsty - 1)  
                        * (long) pair.get(i).second;  
                if (cost < bestCost) {  
                    bestCost = cost;  
                    resultx = pair.get(i).first;  
                    resulty = pair.get(i).second;  
                }  
            }  
            out.write("Case #" + cases + ": " + resultx + " " + resulty + " "  
                    + bestCost + "\n");  
            System.out.print("Case #" + cases + ": " + resultx + " " + resulty  
                    + " " + bestCost + "\n");  
  
        }  
        in.close();  
        out.flush();  
        out.close();  
    }  
}  
```

풀이는 [여기서](http://zhenyiluo.blogspot.kr/2013/12/google-code-jam-notes-meet-and-party.html)가져왔다.

## 해설 

원 풀이가 너무 지저분하다. 정리를 좀 하자.

```java
package codeJam.google.com;  
  
import java.io.File;  
import java.io.FileWriter;  
import java.io.IOException;  
import java.util.ArrayList;  
import java.util.Collections;  
import java.util.Scanner;  
  
/** 
 * @author Zhenyi 2013 Dec 26, 2013 2:17:53 PM 
 * @modified by enshahar to be more readable
 */  
class Pair implements Comparable<Pair> {  
    Integer first;  
    Integer second;  
  
    @Override  
    public int compareTo(Pair o) {  
        int compare = this.first.compareTo(o.first);  
        return compare != 0 ? compare : this.second.compareTo(o.second);  
    }  
  
    public Pair(int x, int y) {  
        this.first = x;  
        this.second = y;  
    }  
  
}  
  
public class MeetAndParty {  
    public static void main(String[] args) throws IOException {  
        Scanner in = new Scanner(new File(  
                "C:/Users/Zhenyi/Downloads/B-small-practice.in"));  
        FileWriter out = new FileWriter(  
                "C:/Users/Zhenyi/Downloads/B-small-practice.out");  
        // Scanner in = new Scanner(new  
        // File("C:/Users/Zhenyi/Downloads/B-large-practice.in"));  
        // FileWriter out = new  
        // FileWriter("C:/Users/Zhenyi/Downloads/B-large-practice.out");  
  
        int T = in.nextInt();  
  
        for (int cases = 1; cases <= T; cases++) {  
            int B = in.nextInt();  
            ArrayList<Integer> x = new ArrayList<Integer>();  
            ArrayList<Integer> y = new ArrayList<Integer>();  
            ArrayList<Pair> pair = new ArrayList<Pair>();  
            for (int i = 0; i < B; i++) {  
                int x1 = in.nextInt();  
                int y1 = in.nextInt();  
                int x2 = in.nextInt();  
                int y2 = in.nextInt();  
                for (int j = x1; j <= x2; j++) {  
                    for (int k = y1; k <= y2; k++) {  
                        x.add(j);  
                        y.add(k);  
                        pair.add(new Pair(j, k));  
                    }  
                }  
            }  
            Collections.sort(x);  
            Collections.sort(y);  
            Collections.sort(pair);  
  
            ArrayList<Long> sumx = new ArrayList<Long>();  
            ArrayList<Long> sumy = new ArrayList<Long>();  
            sumx.add((long) 0);  
            sumy.add((long) 0);  
            for (int i = 0; i < x.size(); i++) {  
                Long tmp = sumx.get(sumx.size() - 1);  
                sumx.add(tmp + x.get(i));  
            }  
            for (int i = 0; i < y.size(); i++) {  
                Long tmp = sumy.get(sumy.size() - 1);  
                sumy.add(tmp + y.get(i));  
            }  
            long bestCost = Long.MAX_VALUE;  
            int resultx = 0;  
            int resulty = 0;
            int n_point = pair.size();
  
            for (int i = 0; i < n_point; i++) {  
                int xpos = pair.get(i).first;
                int ypos = pair.get(i).second;
                int firstx = Collections.binarySearch(x, xpos);  
                int firsty = Collections.binarySearch(y, ypos);  
                long cost_x = (firstx + 1) * (long)xpos
                        - sumx.get(firstx + 1)
                        + sumx.get(n_point) - sumx.get(firstx + 1) 
                        - (n_point - firstx - 1) * (long)xpos 
                long cost_y = (firsty + 1)  * (long)ypos
                        - sumy.get(firsty + 1)
                        + sumy.get(n_point) - sumy.get(firsty + 1)  
                        - (n_point - firsty - 1) * (long)y_pos;  
                if ((cost_x+cost_y) < bestCost) {  
                    bestCost = cost_x + cost_y;  
                    resultx = xpos;  
                    resulty = ypos;  
                }  
            }  
            out.write("Case #" + cases + ": " + resultx + " " + resulty + " "  
                    + bestCost + "\n");  
            System.out.print("Case #" + cases + ": " + resultx + " " + resulty  
                    + " " + bestCost + "\n");  
  
        }  
        in.close();  
        out.flush();  
        out.close();  
    }  
}  
```

## 상세해설

x를 이해하면 y는 완전 같은 로직이다. 따라서 x만 해설한다.

x비용을 계산해 보자. 이 계산을 풀어쓰면 다음과 같다.

```
xpos - x(0) +
xpos - x(1) +
xpos - x(firstx) +
<--- POS1--->
x(firstx+1) - xpos +
x(firstx+2) - xpos +
...
x(n_point-1) - xpos
<--- POS2 -->
```

POS1 위치까지를 보자. 이 위치는 x좌표순으로 정렬시, 현재 검토중인 점의 x좌표와 x좌표가 같은 첫번째 위치까지이다. 개수는 `firstx + 1`개이다. 
마찬가지로 POS2는 전체 `n_point`개의 점에서 POS1까지이미 고려한 점을 제외하면 되므로 개수가 `n_point - firstx - 1`개이다.


이제 각 구간의 합을 계산해보자.

### POS1까지 

```
xpos - x(0) +
xpos - x(1) +
xpos - x(firstx) +
```

1) `xpos`는 `firstx+1`개 있다. 총 합은 `xpos * (firstx+1)`이다.
2) `x(0)`부터 `x(firstx)`까지 더한 것은 누적 값의 리스트인 `sumx`에서 쉽게 찾을 수 있다. 위치는 `firstx+1`번째이다. 따라서, `sumx.get(firstx+1)`이다.
3) 결론적으로 전체 합은 `xpos * (firstx+1) - sumx.get(firstx+1)`이다.

### POS1 직후부터 POS2까지

```
x(firstx+1) - xpos +
x(firstx+2) - xpos +
...
x(n_point-1) - xpos
```
1) `xpos`는 `n_point - firstx - 1`개 있다. 따라서 `xpos * (n_point - firstx - 1)`이다.
2) `x(first+1)+....+x(n_point-1)`은 전체 누적값(`sumx.get(n_point)`)에서 `firstx`위치까지의 누적값(`sumx.get(firstx+1)`)을 빼주면 된다. 따라서, `sumx.get(n_point) - sumx.get(firstx+1)`이다.
3) 결론적으로 전체합은, `sumx.get(n_point) - sumx.get(firstx+1) - xpos * (n_point - firstx - 1)`이다.

이를 정리하면 위 자바코드와 같다.
```java
(firstx + 1) * (long)xpos
- sumx.get(firstx + 1)
+ sumx.get(n_point)- sumx.get(firstx + 1) 
- (n_point - firstx - 1) * (long)xpos 
```

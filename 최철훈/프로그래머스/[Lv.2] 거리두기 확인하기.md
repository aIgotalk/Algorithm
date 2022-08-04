# [LV.2] 거리두기 확인하기

## 서론

이 문제는 보자마자 BFS 알고리즘을 활용해야겠다고 생각한 문제였습니다. 모든 인원에 대해서 사방으로 맨해튼 거리가 2보다 크면서 해당 거리안에 빈 테이블이 아닌 파티션으로 구분되어 있는지를 모두 찾아야 했기 때문입니다. 문제를 어떻게 코드로 작성할건지만 생각하면 생각보다 기본에 충실한 문제였다고 생각합니다.



## **문제설명**

개발자를 희망하는 죠르디가 카카오에 면접을 보러 왔습니다.  

코로나 바이러스 감염 예방을 위해 응시자들은 거리를 둬서 대기를 해야하는데 개발 직군 면접인 만큼  
아래와 같은 규칙으로 대기실에 거리를 두고 앉도록 안내하고 있습니다.

> 1. 대기실은 5개이며, 각 대기실은 5x5 크기입니다.
> 2. 거리두기를 위하여 응시자들 끼리는 맨해튼 거리[1](https://school.programmers.co.kr/learn/courses/30/lessons/81302?language=kotlin#fn1)가 2 이하로 앉지 말아 주세요.
> 3. 단 응시자가 앉아있는 자리 사이가 파티션으로 막혀 있을 경우에는 허용합니다.

예를 들어,

![image](https://user-images.githubusercontent.com/48594786/182874596-3824e93e-ce77-4ce3-a8a3-0aca703b2d29.png)

5개의 대기실을 본 죠르디는 각 대기실에서 응시자들이 거리두기를 잘 기키고 있는지 알고 싶어졌습니다. 자리에 앉아있는 응시자들의 정보와 대기실 구조를 대기실별로 담은 2차원 문자열 배열 `places`가 매개변수로 주어집니다. 각 대기실별로 거리두기를 지키고 있으면 1을, 한 명이라도 지키지 않고 있으면 0을 배열에 담아 return 하도록 solution 함수를 완성해 주세요.



## **입출력 예**

![image](https://user-images.githubusercontent.com/48594786/182874726-55bd107c-70b5-4b11-8f57-1d3ee1623fc5.png)

## 제한사항

- `places`의 행 길이(대기실 개수) = 5
  - `places`의 각 행은 하나의 대기실 구조를 나타냅니다.
- `places`의 열 길이(대기실 세로 길이) = 5
- `places`의 원소는 `P`,`O`,`X`로 이루어진 문자열입니다.
  - `places` 원소의 길이(대기실 가로 길이) = 5
  - `P`는 응시자가 앉아있는 자리를 의미합니다.
  - `O`는 빈 테이블을 의미합니다.
  - `X`는 파티션을 의미합니다.
- 입력으로 주어지는 5개 대기실의 크기는 모두 5x5 입니다.
- return 값 형식
  - 1차원 정수 배열에 5개의 원소를 담아서 return 합니다.
  - `places`에 담겨 있는 5개 대기실의 순서대로, 거리두기 준수 여부를 차례대로 배열에 담습니다.
  - 각 대기실 별로 모든 응시자가 거리두기를 지키고 있으면 1을, 한 명이라도 지키지 않고 있으면 0을 담습니다.
  
  

## 작성한 코드

```kotlin
import java.util.*;

class Solution {
    
    val dirs = arrayOf(
        intArrayOf(0, 1),
        intArrayOf(1, 0),
        intArrayOf(0, -1),
        intArrayOf(-1, 0)
    )

    fun solution(places: Array<Array<String>>): IntArray {
        val answer = IntArray(places.size)
        
        for(index in places.indices) {
            val map = Array(5) { CharArray(5) }
            for (i in 0 until 5) {
                for(j in 0 until 5) {
                    map[i][j] = places[index][i][j]
                }
            }

            answer[index] = bfs(map)
        }

        return answer
    }

    fun bfs(map: Array<CharArray>): Int {
        //응시자 지점 모두 찾기
        val pList = mutableListOf<Node>()
        for (i in 0 until 5) {
            for(j in 0 until 5) {
                if(map[i][j] == 'P') pList.add(Node(i, j, 0))
            }
        }

        //모든 응시자에 대해서 잘 앉아있나 확인하기
        var check = false
        for(p in pList) {
            val queue = LinkedList<Node>()
            val visit = Array(5) { BooleanArray(5) }
            queue.add(Node(p.x, p.y, p.dir))
            visit[p.x][p.y] = true

            while(!queue.isEmpty()) {
                val node = queue.poll()
                
                if(node.dir > 2) continue
                
                for(dir in dirs) {
                    val nextX = node.x + dir[0]
                    val nextY = node.y + dir[1]

                    if(nextX < 0 || nextY < 0 || nextX >= 5 || nextY >= 5) continue //범위내에 없으면 패스
                    if(map[nextX][nextY] == 'X') continue //파티션인 상태 패스
                    if(visit[nextX][nextY]) continue //방문된 곳 패스
                    if(node.dir + 1 <= 2 && map[nextX][nextY] == 'P') { //맨해튼 거리 2를 넘기전에 다른 사람이 있는 경우 
                        check = true
                        break
                    }

                    visit[nextX][nextY] = true
                    queue.add(Node(nextX, nextY, node.dir + 1))
                }

                if(check) break
            }

            if(check) break
        }

        return if(check) 0 else 1
    }

    class Node (
        val x: Int,
        val y: Int,
        val dir: Int
    )
    
}
```

## 

## 접근방법

1. 우선, 입력 places 각각에 대해 2차원 배열을 생성해주었습니다. 각각의 2차원 배열을 가지고 BFS 알고리즘을 활용하기 위해서 입니다.

2. 각 2차원 배열에 대해서 bfs 메서드를 실행해줍니다.
   
   2-1) BFS 알고리즘을 활용하기 위해선 자료구조 Queue가 필요한데 여기에 기준이 되는 데이터를 넣어야 합니다. 이 문제에서는 응시자들을 기준으로 생각하고 데이터를 넣어주었습니다. 왜냐하면 각 응시자들이 다른 응시자들과 거리두기가 잘 되어있는지를 확인해야하기 때문입니다.
   
   2-2) Queue에 저장된 데이터를 하나씩 꺼내어 (x, y) 좌표를 이동시키며 필요한 조건에 대해서 확인해보도록 합니다.
   
   2-3) (x, y) 좌표가 범위내에 있지 않은 경우 패스합니다.
   
   2-4) (x, y) 좌표가 파티션이면 패스합니다.
   
   2-5) 이미 방문된 곳이라면 패스합니다.
   
   2-6) 맨해튼 거리가 2이하인 와중에 (x, y) 좌표가 다른 응시자이면 거리두기가 되어있지 않은 것이기 때문에 해당 bfs 메서드를 종료하며 0을 반환합니다.
   
   2-7) 그렇지 않은 경우는 위의 조건을 모두 만족하지 않기 때문에 Queue에 다음 좌표에 대한 정보를 저장합니다.
   
   2-8) 위의 과정을 반복하여 아무 문제가 없을 경우는 거리두기가 잘 지켜지고 있는 것이기 때문에 1을 반환하도록 합니다.

3. 2번에서의 반환 값을 배열로 묶어 반환하도록 합니다.



## 결론

제가 개인적으로 좋아하는 BFS 알고리즘 문제여서 조금 더 집중해서 풀 수 있었습니다. BFS 문제를 풀때마다 느끼는 거지만, 일반적으로는 BFS 알고리즘이 구성하고자 하는 큰 틀은 벗어나지 않는 것 같습니다. 다만, DFS나 백트래킹 같은 알고리즘을 활용한다던가 문제의 요구사항을 어떻게 BFS 알고리즘을 활용할지를 떠올리는게 가끔은 어렵게 느껴지기도 합니다. 그래도 이러한 문제 난이도 같은 경우에는 실전에서도 빠르게 풀 수 있어야합니다.

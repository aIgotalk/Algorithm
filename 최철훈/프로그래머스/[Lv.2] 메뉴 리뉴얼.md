# [LV.2] 메뉴 리뉴얼

## 서론

이 문제는 큰틀에서 DFS 알고리즘으로 해결하면 되는 문제라 큰 어려움은 없었지만, 제가 선택한 방법내에서 자료구조를 활용해야만 쉽게 해결이 가능 하였고, 알고리즘을 어떻게 작성할지도 고민이 필요했고 추가적으로, 문제 뿐만 아니라 입출력 예제까지도 확인해야 완벽히 풀이할 수 있는 문제였습니다. 


## **문제설명**

레스토랑을 운영하던 `스카피`는 코로나19로 인한 불경기를 극복하고자 메뉴를 새로 구성하려고 고민하고 있습니다.  
기존에는 단품으로만 제공하던 메뉴를 조합해서 코스요리 형태로 재구성해서 새로운 메뉴를 제공하기로 결정했습니다. 어떤 단품메뉴들을 조합해서 코스요리 메뉴로 구성하면 좋을 지 고민하던 "스카피"는 이전에 각 손님들이 주문할 때 가장 많이 함께 주문한 단품메뉴들을 코스요리 메뉴로 구성하기로 했습니다.  
단, 코스요리 메뉴는 최소 2가지 이상의 단품메뉴로 구성하려고 합니다. 또한, 최소 2명 이상의 손님으로부터 주문된 단품메뉴 조합에 대해서만 코스요리 메뉴 후보에 포함하기로 했습니다.

예를 들어, 손님 6명이 주문한 단품메뉴들의 조합이 다음과 같다면,  
(각 손님은 단품메뉴를 2개 이상 주문해야 하며, 각 단품메뉴는 A ~ Z의 알파벳 대문자로 표기합니다.)

| 손님 번호 | 주문한 단품메뉴 조합   |
| ----- | ------------- |
| 1번 손님 | A, B, C, F, G |
| 2번 손님 | A, C          |
| 3번 손님 | C, D, E       |
| 4번 손님 | A, C, D, E    |
| 5번 손님 | B, C, F, G    |
| 6번 손님 | A, C, D, E, H |

가장 많이 함께 주문된 단품메뉴 조합에 따라 "스카피"가 만들게 될 코스요리 메뉴 구성 후보는 다음과 같습니다.

| 코스 종류    | 메뉴 구성      | 설명                                 |
| -------- | ---------- | ---------------------------------- |
| 요리 2개 코스 | A, C       | 1번, 2번, 4번, 6번 손님으로부터 총 4번 주문됐습니다. |
| 요리 3개 코스 | C, D, E    | 3번, 4번, 6번 손님으로부터 총 3번 주문됐습니다.     |
| 요리 4개 코스 | B, C, F, G | 1번, 5번 손님으로부터 총 2번 주문됐습니다.         |
| 요리 4개 코스 | A, C, D, E | 4번, 6번 손님으로부터 총 2번 주문됐습니다.         |

각 손님들이 주문한 단품메뉴들이 문자열 형식으로 담긴 배열 orders, "스카피"가 `추가하고 싶어하는` 코스요리를 구성하는 단품메뉴들의 갯수가 담긴 배열 course가 매개변수로 주어질 때, "스카피"가 새로 추가하게 될 코스요리의 메뉴 구성을 문자열 형태로 배열에 담아 return 하도록 solution 함수를 완성해 주세요.



## **입출력 예**

![image](https://user-images.githubusercontent.com/48594786/182403977-8458b2b6-1438-433b-a201-5d3ee6cef46d.png)

## 

## 제한사항

- orders 배열의 크기는 2 이상 20 이하입니다.
- orders 배열의 각 원소는 크기가 2 이상 10 이하인 문자열입니다.
  - 각 문자열은 알파벳 대문자로만 이루어져 있습니다.
  - 각 문자열에는 같은 알파벳이 중복해서 들어있지 않습니다.
- course 배열의 크기는 1 이상 10 이하입니다.
  - course 배열의 각 원소는 2 이상 10 이하인 자연수가 `오름차순`으로 정렬되어 있습니다.
  - course 배열에는 같은 값이 중복해서 들어있지 않습니다.
- 정답은 각 코스요리 메뉴의 구성을 문자열 형식으로 배열에 담아 사전 순으로 `오름차순` 정렬해서 return 해주세요.
  - 배열의 각 원소에 저장된 문자열 또한 알파벳 `오름차순`으로 정렬되어야 합니다.
  - 만약 가장 많이 함께 주문된 메뉴 구성이 여러 개라면, 모두 배열에 담아 return 하면 됩니다.
  - orders와 course 매개변수는 return 하는 배열의 길이가 1 이상이 되도록 주어집니다.

## 

## 작성한 코드

```kotlin
class Solution {

    private val courseMap = hashMapOf<String, Int>()
    private var courseMaxCount: Int = Integer.MIN_VALUE

    fun solution(orders: Array<String>, course: IntArray): Array<String> {
        val result = mutableListOf<String>()
        
        course.forEach { num ->
            //초기화
            courseMap.clear()
            courseMaxCount = Integer.MIN_VALUE
            
            orders.forEach { order ->
                dfs(num, 0, -1, order, "")
            }
            
            courseMap.filter {
                it.value == courseMaxCount && it.value > 1
            }.forEach {
                result.add(it.key)
            }
        }
        
        return result
            .sorted()
            .toTypedArray()
    } 
    
    private fun dfs(
        limit: Int,
        depth: Int,
        next: Int,
        order: String,
        nextOrder: String
    ) {
        if(limit == depth) {
            val combination = nextOrder.toCharArray().sorted().joinToString("")
            
            courseMap[combination] = courseMap.getOrDefault(combination, 0) + 1            
            courseMap[combination]?.run {
                if(this > courseMaxCount) {
                    courseMaxCount = courseMap[combination]!!
                }
            }
            
            return
        }
        
        for(index in order.indices) {
            if(index > next) {
                dfs(limit, depth + 1, index, order, nextOrder + order[index])
            }
        }
    }

}
```

## 

## 접근방법

1. 우선, 각 course에 대해 카운팅 할 목적으로 courseMap이라는 변수를 선언하였습니다.
   
   1-1)  courseMaxCount는 각 course 별 가장 주문이 많이 된 코스를 확인하기 위해 선언하였습니다.

2. course 개수를 기준으로 반복문을 실행합니다. 모든 orders에 대해서 course 2개 짜리, 3개 짜리 .. 와 같이 진행합니다.
   
   2-1) 여기서 각 course 개수에 해당하는 모든 orders에 대해 조합을 구하기 위해, DFS 알고리즘을 사용하였습니다. 모든 orders[index]의 값들에서의 조합을 구하였습니다. 작성된 dfs 메서드는 예를 들면, orders[index]의 값이 "ABCFG", course가 2이면 AB, AC, AF, AG, BC, BF, BG, CF, CG, FG와 같이 모든 조합의 수를 구하는 과정입니다. 
   
   2-2) DFS의 종료 지점에서 선택된 문자열을 꼭 정렬하여 courseMap의 key로 사용하도록 합니다. 문제에서는 해당 예외 처리에 대해 언급이 없었지만, 입출력으로 주어진 3번째 입력값을 보면 orders[2]에 해당하는 WXA 값이 있습니다. 실제로 WXA와 AWS의 course 갯수 만큼의 조합을 구하고 key로 사용하는 것은 완전히 다르기 때문입니다. 실제로 정렬을 하지 않고 코드를 제출하면 해결되지 않는 테스트 케이스가 발생하기도 합니다.

3. 각 course 개수에 해당하는 모든 조합을 구했으면 courseMap에 저장된 모든 값들중에서 courseMaxCount의 개수와 같은 모든 값들을 필터링 합니다. 
   
   3-1) 여기서 추가적으로 주의해야할 부분이 있습니다. 각 조합에 대한 값이 2이상이어야 합니다. 왜냐하면 문제에서, 최소 2명 이상의 손님으로부터 주문된 단품메뉴 조합에 대해서만 코스요리 메뉴 후보에 포함한다고 언급했기 때문입니다.
   
   3-2) 필터링 한 후에 key로 사용된 후보 메뉴를 결과값으로 반환할 result 변수에 추가합니다. 

4. 위의 2, 3번 과정을 반복하여 저장된 result에 대해서 오름차순으로 정렬하여 반환합니다.

## 

## 결론

이 문제는 DFS 알고리즘에 포인트가 맞춰졌기 보다는 어떻게 요구사항들을 알고리즘으로 구성해 나갈지가 조금 더 돋보였던 문제였습니다. 또한, 접근방법 2-2)와 3-1)에서 말씀드린 것 처럼 문제와 입출력 예제를 모두 꼼꼼히 읽어야지만 보이는 예외 처리가 눈에 띄는 문제였습니다. 문제는 간단하지만, 실전에서 앞서 말씀드린 사항을 모두 파악하는 것이 생각보다 어려울 수도 있겠습니다. 그래서 우리는 평소에 습관을 잘 들이고 꼼꼼하게 문제를 읽어야 할 필요가 있습니다!

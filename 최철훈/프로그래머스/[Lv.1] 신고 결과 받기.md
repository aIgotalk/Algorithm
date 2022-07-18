# [LV.1] 신고 결과 받기

## 서론

이 문제는 자료구조를 얼마나 잘 활용하고 이해하고 있는지를 파악해보려는 의도가 돋보이는 문제였습니다. 그리고 제가 작성한 알고리즘의 시간 복잡도로 충분히 풀릴줄 알았지만, 실행 결과중 3개가 시간 초과가 발생하여 한 번에 성공하지는 못했습니다. 하지만, 개선점을 고민하다가 메모리 공간을 조금 더 활용하여 시간 복잡도를 낮추어 문제를 빠르게 해결할 수 있었습니다. 문제를 해결하고 나니 굳이 왜 id_list를 파라미터로 주었는지 알 수 있었던 문제입니다.



## **문제설명**

신입사원 무지는 게시판 불량 이용자를 신고하고 처리 결과를 메일로 발송하는 시스템을 개발하려 합니다. 무지가 개발하려는 시스템은 다음과 같습니다.

- 각 유저는 한 번에 한 명의 유저를 신고할 수 있습니다.
  - 신고 횟수에 제한은 없습니다. 서로 다른 유저를 계속해서 신고할 수 있습니다.
  - 한 유저를 여러 번 신고할 수도 있지만, 동일한 유저에 대한 신고 횟수는 1회로 처리됩니다.
- k번 이상 신고된 유저는 게시판 이용이 정지되며, 해당 유저를 신고한 모든 유저에게 정지 사실을 메일로 발송합니다.
  - 유저가 신고한 모든 내용을 취합하여 마지막에 한꺼번에 게시판 이용 정지를 시키면서 정지 메일을 발송합니다.

다음은 전체 유저 목록이 ["muzi", "frodo", "apeach", "neo"]이고, k = 2(즉, 2번 이상 신고당하면 이용 정지)인 경우의 예시입니다.

| 유저 ID    | 유저가 신고한 ID | 설명                         |
| -------- | ---------- | -------------------------- |
| "muzi"   | "frodo"    | "muzi"가 "frodo"를 신고했습니다.   |
| "apeach" | "frodo"    | "apeach"가 "frodo"를 신고했습니다. |
| "frodo"  | "neo"      | "frodo"가 "neo"를 신고했습니다.    |
| "muzi"   | "neo"      | "muzi"가 "neo"를 신고했습니다.     |
| "apeach" | "muzi"     | "apeach"가 "muzi"를 신고했습니다.  |

각 유저별로 신고당한 횟수는 다음과 같습니다.

| 유저 ID    | 신고당한 횟수 |
| -------- | ------- |
| "muzi"   | 1       |
| "frodo"  | 2       |
| "apeach" | 0       |
| "neo"    | 2       |

위 예시에서는 2번 이상 신고당한 "frodo"와 "neo"의 게시판 이용이 정지됩니다. 이때, 각 유저별로 신고한 아이디와 정지된 아이디를 정리하면 다음과 같습니다.

| 유저 ID    | 유저가 신고한 ID        | 정지된 ID           |
| -------- | ----------------- | ---------------- |
| "muzi"   | ["frodo", "neo"]  | ["frodo", "neo"] |
| "frodo"  | ["neo"]           | ["neo"]          |
| "apeach" | ["muzi", "frodo"] | ["frodo"]        |
| "neo"    | 없음                | 없음               |

따라서 "muzi"는 처리 결과 메일을 2회, "frodo"와 "apeach"는 각각 처리 결과 메일을 1회 받게 됩니다.

이용자의 ID가 담긴 문자열 배열 `id_list`, 각 이용자가 신고한 이용자의 ID 정보가 담긴 문자열 배열 `report`, 정지 기준이 되는 신고 횟수 `k`가 매개변수로 주어질 때, 각 유저별로 처리 결과 메일을 받은 횟수를 배열에 담아 return 하도록 solution 함수를 완성해주세요.

## **입출력 예**

![image](https://user-images.githubusercontent.com/48594786/179525943-9f2701a1-f8cb-4fa8-80ba-8e64ecd5c2f9.png)

## 제한사항

- 2 ≤ `id_list`의 길이 ≤ 1,000
  - 1 ≤ `id_list`의 원소 길이 ≤ 10
  - `id_list`의 원소는 이용자의 id를 나타내는 문자열이며 알파벳 소문자로만 이루어져 있습니다.
  - `id_list`에는 같은 아이디가 중복해서 들어있지 않습니다.
- 1 ≤ `report`의 길이 ≤ 200,000
  - 3 ≤ `report`의 원소 길이 ≤ 21
  - `report`의 원소는 "이용자id 신고한id"형태의 문자열입니다.
  - 예를 들어 "muzi frodo"의 경우 "muzi"가 "frodo"를 신고했다는 의미입니다.
  - id는 알파벳 소문자로만 이루어져 있습니다.
  - 이용자id와 신고한id는 공백(스페이스)하나로 구분되어 있습니다.
  - 자기 자신을 신고하는 경우는 없습니다.
- 1 ≤ `k` ≤ 200, `k`는 자연수입니다.
- return 하는 배열은 `id_list`에 담긴 id 순서대로 각 유저가 받은 결과 메일 수를 담으면 됩니다.

## 작성한 코드

```kotlin
class Solution {

    fun solution(
        id_list: Array<String>, 
        report: Array<String>, 
        k: Int
    ): IntArray {
        val answer = IntArray(id_list.size) { 0 }
        
        val idMap = hashMapOf<String, Int>()
        id_list.forEachIndexed { index, user ->
            idMap[user] = index
        }
        
        val reportUserMap = hashMapOf<String, Int>()
        val reportDuplicateSet = hashSetOf<String>()
        report.forEach { 
            if(reportDuplicateSet.contains(it).not()) {
                reportDuplicateSet.add(it)
                val info = it.split(" ")
                val target = info[1]
                reportUserMap[target] = reportUserMap.getOrDefault(target, 0) + 1
            }
        }
        
        val reportSet = reportUserMap.filter {
            it.value >= k
        }.map {
            it.key
        }.toSet()
        
        reportDuplicateSet.clear()
        
        report.forEach {
            val info = it.split(" ")
            val name = info[0]
            val target = info[1]

            if(reportSet.contains(target)) {
                if(reportDuplicateSet.contains(it).not()) {
                    reportDuplicateSet.add(it)
                    answer[idMap[name]!!]++
                }
            }
        }
        
        return answer
    }

}
```

## 접근방법

1. 우선, 문제의 제한사항을 읽어보고 report 배열의 길이에 주의해야 합니다. 길이가 최대 20만이기 때문에 report를 기준으로 2중 for문을 돌리면 안되겠다는 생각이 먼저 들어야합니다.
   
   1-1) 추가적으로, id_list 배열의 길이가 최대 1000이기도 합니다. id_list X report 만큼의 연산이 수행된다고 생각하면 최대 배열길이 기준 최소 2억번의 연산을 진행하게 될 것입니다. 보통 1억번의 연산을 1초로 가정하기 때문에, 이런식으로 알고리즘을 작성하면 2초 이상이 걸릴 수 있음을 유의하여야 합니다. 위의 코드로 문제를 해결하기전에 저는 id_list x report 만큼의 연산을 수행하였더니 3개의 문제에서 시간 초과가 났습니다. 따라서 개선된 코드를 알아보도록 합니다.

2. idMap이라는 변수를 정의하였습니다. 여기서는 각 id에 대한 index 번호를 저장하도록 합니다. 이 변수를 정의한 이유는 아래와 같습니다.
   
   2-1) 반환값을 미리 확인해보면, id_list의 각 index에 카운팅 한 값을 반환해야 하기 때문에, 반환 값이 추후에 설정되기 위해서는 id에 대한 index 번호가 필요합니다.
   
   2-2) 위의 1-1)에서 언급한 것처럼 id_list x report의 연산 횟수를 줄이기 위해서입니다. 아래 5-3)의 문장을 확인하시면 됩니다.

3. report를 기준으로 누가 누구를 신고했는지 확인해야 합니다.
   
   3-1) 여기서 주의해야 할 점이 있습니다. 문제에서 한 유저를 여러 번 신고할 수도 있지만, 동일한 유저에 대한 신고 횟수는 1회로 처리하라고 했습니다. 그래서 Set 자료구조를 사용하여 reportDuplicateSet 변수를 정의하였습니다. report[index] 값이 동일할 때 하나의 값만을 활용하기 위해서입니다. report 값을 확인한 이후에 사용된 것은 reportDuplicateSet에 추가하도록 합니다. 이후에 reportDuplicateSet에 report[index]값이 포함되어 있는지 확인하고, 이전에 추가된 신고내용이면 패스하도록 합니다.
   
   3-2) reportUserMap에 신고당한 유저의 ID와 신고 횟수를 증가시키도록 합니다.
   
   3-3) 신고 횟수를 Value로 설정한 이유는 신고 횟수가 파라미터 k이상일 때 신고당한 유저의 ID만 추려내기 위해서입니다. 
   
   3-4) report의 내용을 모두 확인 후 필요한 값들을 저장했으면, 신고 횟수가 k이상인 유저들만 필터링을 진행하고 key부분만 추출하여 confirmedReportSet에 저장합니다.                                                                          

4. 앞에 report 내용을 확인하기 위해 사용했던 reportDuplicateSet을 다시 초기화해줍니다. (다른 Set 변수를 선언해도 되지만 재 사용하기 위해서)

5. report 내용을 다시 한 번 확인합니다.
   
   5-1) confirmedReportSet에 포함된 유저인지 확인합니다. 포함되어있다는 것은 k번이상 신고당한 유저라는 의미입니다.
   
   5-2) 3-1) 내용과 동일합니다.
   
   5-3) 누가 신고했는지를 카운팅해줘야 하기 때문에, 1번에서 설정한 idMap을 활용하여 신고한 유저의 index 번호를 알아냅니다. answer[신고한 유저의 index]에 값을 카운팅하여 반환합니다. (id_list X report 크기의 연산횟수로 반환 값을 구하던 것을 report의 크기의 연산횟수만으로 찾을 수 있게 되었습니다)
   
   

## 결론

이 문제는 한 번에 통과하지 못해서 조금 실망스러웠지만(?) 그래도 어떤 부분이 문제인지 찾아보다 개선점을 발견하여 빠르게 해결할 수 있었습니다. 알고리즘 문제 풀이중에 이런 고민에 빠져 코드를 개선하는건 늘 흥미롭게 느껴지기도 합니다. 문제는 크게 복잡한 것은 없었고, 주어진 설명을 꼼꼼히 읽어야지만 어떤 자료구조를 활용할 수 있을지 생각할 수 있는 문제였습니다.

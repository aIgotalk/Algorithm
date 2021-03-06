# [LV.2] 양궁대회

## 서론

프로그래머스에서 양궁대회 문제를 처음 풀어보았습니다. 문제 자체를 이해하는 것은 크게 어렵지 않았습니다.  이 문제는 저희가 기본적으로 알고있는 DFS 알고리즘과 백트래킹 기술을 사용하여 문제에 쉽게 접근할 수 있습니다. 하지만, 추가적인 아이디어를 생각해야지만 제한된 시간내에 알고리즘이 정상 동작하게 해결할 수 있었습니다. DFS 알고리즘과 백트래킹으로만 해결하려한다면 시간초과가 되거나 원하는 답을 찾지 못할수도 있습니다.



## **문제설명**

![image](https://user-images.githubusercontent.com/48594786/178754029-60b22a3e-0284-4f50-b390-b57fe918c475.png)

카카오배 양궁대회가 열렸습니다.  
`라이언`은 저번 카카오배 양궁대회 우승자이고 이번 대회에도 결승전까지 올라왔습니다. 결승전 상대는 `어피치`입니다.  
카카오배 양궁대회 운영위원회는 한 선수의 연속 우승보다는 다양한 선수들이 양궁대회에서 우승하기를 원합니다. 따라서, 양궁대회 운영위원회는 결승전 규칙을 전 대회 우승자인 라이언에게 불리하게 다음과 같이 정했습니다.

1. 어피치가 화살 `n`발을 다 쏜 후에 라이언이 화살 `n`발을 쏩니다.
2. 점수를 계산합니다.
   1. 과녁판은 아래 사진처럼 생겼으며 가장 작은 원의 과녁 점수는 10점이고 가장 큰 원의 바깥쪽은 과녁 점수가 0점입니다.
   2. 만약, k(k는 1~10사이의 자연수)점을 어피치가 a발을 맞혔고 라이언이 b발을 맞혔을 경우 더 많은 화살을 k점에 맞힌 선수가 k 점을 가져갑니다. 단, a = b일 경우는 어피치가 k점을 가져갑니다. **k점을 여러 발 맞혀도 k점 보다 많은 점수를 가져가는 게 아니고 k점만 가져가는 것을 유의하세요. 또한 a = b = 0 인 경우, 즉, 라이언과 어피치 모두 k점에 단 하나의 화살도 맞히지 못한 경우는 어느 누구도 k점을 가져가지 않습니다.
      - 예를 들어, 어피치가 10점을 2발 맞혔고 라이언도 10점을 2발 맞혔을 경우 어피치가 10점을 가져갑니다.
      - 다른 예로, 어피치가 10점을 0발 맞혔고 라이언이 10점을 2발 맞혔을 경우 라이언이 10점을 가져갑니다.
   3. 모든 과녁 점수에 대하여 각 선수의 최종 점수를 계산합니다.
3. 최종 점수가 더 높은 선수를 우승자로 결정합니다. 단, 최종 점수가 같을 경우 어피치를 우승자로 결정합니다.
   
   현재 상황은 어피치가 화살 `n`발을 다 쏜 후이고 라이언이 화살을 쏠 차례입니다.  
   라이언은 어피치를 가장 큰 점수 차이로 이기기 위해서 `n`발의 화살을 어떤 과녁 점수에 맞혀야 하는지를 구하려고 합니다.
   
   
   
   화살의 개수를 담은 자연수 `n`, 어피치가 맞힌 과녁 점수의 개수를 10점부터 0점까지 순서대로 담은 정수 배열 `info`가 매개변수로 주어집니다. 이때, 라이언이 가장 큰 점수 차이로 우승하기 위해 `n`발의 화살을 어떤 과녁 점수에 맞혀야 하는지를 10점부터 0점까지 순서대로 정수 배열에 담아 return 하도록 solution 함수를 완성해 주세요. 만약, 라이언이 우승할 수 없는 경우(무조건 지거나 비기는 경우)는 `[-1]`을 return 해주세요.
   
   

## **입출력 예**

![image](https://user-images.githubusercontent.com/48594786/178753978-afab9968-7d30-420f-91ec-b9b71e83b460.png)

 

## 제한사항

- 1 ≤ `n` ≤ 10
- `info`의 길이 = 11
  - 0 ≤ `info`의 원소 ≤ `n`
  - `info`의 원소 총합 = `n`
  - `info`의 i번째 원소는 과녁의 `10 - i` 점을 맞힌 화살 개수입니다. ( i는 0~10 사이의 정수입니다.)
- 라이언이 우승할 방법이 있는 경우, return 할 정수 배열의 길이는 11입니다.
  - 0 ≤ return할 정수 배열의 원소 ≤ `n`
  - return할 정수 배열의 원소 총합 = `n` (꼭 n발을 다 쏴야 합니다.)
  - return할 정수 배열의 i번째 원소는 과녁의 `10 - i` 점을 맞힌 화살 개수입니다. ( i는 0~10 사이의 정수입니다.)
  - **라이언이 가장 큰 점수 차이로 우승할 수 있는 방법이 여러 가지 일 경우, 가장 낮은 점수를 더 많이 맞힌 경우를 return 해주세요.**
    - 가장 낮은 점수를 맞힌 개수가 같을 경우 계속해서 그다음으로 낮은 점수를 더 많이 맞힌 경우를 return 해주세요.
    - 예를 들어, `[2,3,1,0,0,0,0,1,3,0,0]`과 `[2,1,0,2,0,0,0,2,3,0,0]`를 비교하면 `[2,1,0,2,0,0,0,2,3,0,0]`를 return 해야 합니다.
    - 다른 예로, `[0,0,2,3,4,1,0,0,0,0,0]`과 `[9,0,0,0,0,0,0,0,1,0,0]`를 비교하면`[9,0,0,0,0,0,0,0,1,0,0]`를 return 해야 합니다.
- 라이언이 우승할 방법이 없는 경우, return 할 정수 배열의 길이는 1입니다.
  - 라이언이 어떻게 화살을 쏘든 **라이언의 점수가 어피치의 점수보다 낮거나 같으면 `[-1]`을 return 해야 합니다.**

## 

## 작성한 코드

```kotlin
class Solution {
    
    var ryan = IntArray(11) { 0 }
    var answer = IntArray(1) { -1 }
    var max = 0

    fun solution(n: Int, info: IntArray): IntArray {
        dfs(n, 0, info)

        return answer
    }

    fun dfs(n: Int, depth: Int, info: IntArray) {
        if(depth > n) return
        if(depth == n) {
            var sumRyan = 0
            var sumApeach = 0
            for(index in 0 .. 10) {
                if(ryan[index] != 0 || info[index] != 0) {
                    if(ryan[index] > info[index]) {
                        sumRyan += (10 - index)
                    } else {
                        sumApeach += (10 - index)
                    }
                }
            }

            if(sumRyan > sumApeach && (sumRyan - sumApeach) >= max) {
                max = sumRyan - sumApeach
                answer = ryan.clone()
            }

            return
        }

        for(index in 0 .. 10) {
            if(index == 10) {
                ryan[10] += (n - depth)
                dfs(n, n, info)
                ryan[10] -= (n - depth)
            } else {
                if(ryan[index] <= info[index]) {
                    val diff = info[index] - ryan[index]
                    ryan[index] += (diff + 1)
                    dfs(n, depth + (diff + 1), info)
                    ryan[index] -= (diff + 1)
                }
            }
        }
    }
    
}
```

## 

## 접근방법

1. 라이언으로 사용할 ryan 배열과 반환 값으로 사용할 answer 배열을 클래스 멤버 변수로 준비합니다.

2. 바로 DFS 알고리즘과 백트래킹 기술을 사용한 코드를 확인해보겠습니다.
   
   2-1) 우선 종료문을 보기전에 반복문을 통해 어떻게 깊이 우선 탐색을 하는지 확인해봅시다. 여기서, 두 가지 확인할 점이 있습니다.
   
   2-2) ryan[index]값이 info[index]값보다 작거나 같을 경우 그 차이값보다 1큰 수를 배열값에 저장해주고 있습니다. 그 이유는, 문제에 나와있습니다. 라이언이 어피치가 맞춘 과녁보다 1개이상 맞춰야 점수를 가져올 수 있고, 가장 높은 점수차이로 이기고자 한다면 라이언이 화살을 최대한 아끼며 효율적으로 맞춰야합니다. 그렇기 때문에 어피치가 맞춘 과녁보다 1개 더 맞췄다고 가정하고 문제를 해결해야 합니다.
   
   2-3) index가 10일 때와 10이 아닐때를 구분해주고 있습니다. 그 이유는, DFS의 종료문에 진입하여야 하기 때문입니다. 종료문에 진입하지 못하면, 어피치와 라이언의 점수계산을 할 수가 없어서 원하는 답을 놓칠 수도 있기 때문에, 강제로 진입시켜야합니다. 

3. 그런 다음 DFS 종료문을 확인해봅시다.
   
   3-1) 1. if(depth > n) return 이란 반복문이 왜 필요할지 생각해봐야 합니다. 아래 for문에서 if(ryan[index] <= info[index]) 조건문을 통해 다음의 depth가 depth + (diff + 1) 값이 되는데, 문제를 보면 화살을 n방만 쐈기 때문에, n방 이상을 쐈으면 과감하게 버려야합니다.
   
   3-2) 2. if(depth == n) 조건문은 라이언도 n방의 화살을 쐈고, 이제는 어피치와 라이언의 점수를 비교해야 합니다. 비교하여 점수를 비교하는 로직은 생각보다 쉽습니다. 근데 여기서 더 생각해야 할부분은, 문제에서는 분명 라이언이 가장 큰 점수 차이로 우승할 수 있는 방법이 여러가지라면, 가장 낮은 점수를 더 많이 맞힌 경우를 return해달라고 했습니다. 위의 코드로 어떻게 이 케이스를 통과시킬 수 있을까요?         
   
   이해를 해보면 생각보다 쉽습니다. 예를 들면 (1, 2, 3, 4) 배열에서 2개씩 뽑는다고 가정할 때, 위의 알고리즘대로 앞에서부터 차근차근 뽑아보면 (1, 2), (1, 3), (1, 4), (2, 3), (2, 4), (3, 4)와 같이 뽑을 수 있습니다. if(depth == n)의 조건문에 (2, 4), (3, 4)와 같은 배열 값보다 (1, 2), (1, 3) 배열이 먼저 들어와서 조건문을 진행하게 됩니다. 같은 조건과 답이라면 가장 마지막에 있는 (3, 4)가 답으로 반환된다 생각하면 문제를 이해하기 조금 더 나을수도 있습니다. 이런 과정을 이해하는게 생각보다 까다로울 수 있지만, 위의 예시와 비슷한 문맥으로 이해하다보면 금방 이해가 될 것 입니다.

## 

## 결론

이 문제는 읽어보면 어떤 알고리즘을 적용해야 하는지는 짧은 시간내에 생각해볼 수 있을 것 입니다. 하지만, 예외 케이스를 얼마나 빠르게 찾느냐의 문제인 것 같습니다.

저도 나름대로 DFS 알고리즘이나 백트래킹을 활용해야하는 문제에 자신이 있었지만, 예외적인 케이스를 찾아내는데에 시간을 좀 쏟은 것 같습니다. 하지만 구성된 코드를 확인해보면 DFS 알고리즘의 가장 기본적인 뼈대는 갖추고 있는 것을 확인할 수 있었습니다. 

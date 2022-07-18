# [LV.1] 소수 만들기

## 서론

이 문제는 DFS 알고리즘을 학습하고 적용해볼 수 있는 간단한 문제입니다. 추가적으로, 소수를 판별할 수 있는 알고리즘을 실제 코드로 작성할 수 있어야 합니다.



## **문제설명**

주어진 숫자 중 3개의 수를 더했을 때 소수가 되는 경우의 개수를 구하려고 합니다. 숫자들이 들어있는 배열 nums가 매개변수로 주어질 때, nums에 있는 숫자들 중 서로 다른 3개를 골라 더했을 때 소수가 되는 경우의 개수를 return 하도록 solution 함수를 완성해주세요.

## **입출력 예**

![image](https://user-images.githubusercontent.com/48594786/179548945-f236c214-2a50-47ae-a90c-70562dff8236.png)

## 제한사항

- nums에 들어있는 숫자의 개수는 3개 이상 50개 이하입니다.
- nums의 각 원소는 1 이상 1,000 이하의 자연수이며, 중복된 숫자가 들어있지 않습니다.

## 작성한 코드

```kotlin
import kotlin.math.*

class Solution {
    
    lateinit var check: BooleanArray
    lateinit var selectedNums: IntArray
    var answer = 0
    
    fun solution(nums: IntArray): Int { 
        check = BooleanArray(nums.size) { false }
        selectedNums = IntArray(3) { 0 }
        
        dfs(0, 0, nums)
        
        return answer
    }
    
    private fun dfs(
        depth: Int,
        next: Int,
        nums: IntArray
    ) {
        if(depth == 3) {
            if(isPrime(selectedNums.sum())) 
                answer++
            
            return
        }
        
        nums.forEachIndexed { index, num ->
            if(check[index].not() && index >= next) {
                check[index] = true
                selectedNums[depth] = num
                dfs(depth + 1, index + 1, nums)
                selectedNums[depth] = 0
                check[index] = false
            }
        }
    }
    
    private fun isPrime(sum: Int): Boolean {
        if(sum <= 1) return false
        
        for(num in 2 .. sqrt(sum.toDouble()).toInt()) {
            if(sum % num == 0) return false

        }
        
        return true
    }

}
```

## 접근방법

1. nums의 크기와 동일한 check 배열, 숫자 3개를 고를 selectedNums 배열을 선언합니다.

2. DFS 알고리즘을 통해 nums 배열을 탐색하여 고른 숫자를 selectedNums에 index 0부터 순차적으로 저장합니다.
   
   2-1) dfs 메서드를 보면 종료 지점과 탐색 지점이 있습니다.
   
   2-2) 우선, nums 배열을 기준으로 탐색을 시작합니다. check 배열은 nums의 배열값 중 현재 nums[index] 값이 선택되었는지 확인합니다. 선택되어있지 않다면 check[index]를 true로 만들어 선택했다고 표시합니다. selectedNums[depth]에 해당 값을 저장합니다. 다음 dfs를 시작할때는, index에 해당하는 값은 이미 저장되어 있기 때문에 index + 1 부터 탐색하면 됩니다. 또한, 다음 값을 저장할 때는 selectedNums[depth + 1]에 저장하면 됩니다.
   
   2-3) if(depth == 3)의 의미는 selectedNums의 0, 1, 2 인덱스에 모든 값이 저장되었음을 의미합니다. 그렇기에 selectedNums의 값들을 모두 더한 숫자가 소수인지 판별합니다. 만약 소수라면 answer에 값을 카운팅합니다.

3. 소수를 판별하기 위한 알고리즘은 여러가지가 있습니다만, 조금 더 효율적인 소수 판별 알고리즘을 사용하였습니다.
   
   3-1) 우선 소수는 1과 자기 자신만을 약수로 가지는 숫자를 말합니다. 소수를 판별할 때, 2 .. sqrt(sum) 범위까지만 돌아도 판별할 수 있는 이유는, N을 어떤 숫자로 나눈다면 몫이 생기는데 몫과 나누는 숫자중 하나는 N 제곱근 이하이기 때문에 N 제곱근 이하의 숫자가 N과 나눠지는지만 판별하면 N이 소수인지 알 수 있기 때문입니다. (나눠진다면 1과 N 말고도 약수를 추가적으로 가진다는 것을 의미합니다)
   
   

## 결론

이 문제는 기본적인 DFS 알고리즘을 활용하여 연습하기에 좋은 문제입니다. 간단하기 때문에 DFS가 어떤식으로 동작하는지 직접 그려보며 확인하면 큰 도움이 될 것 입니다. 추가적으로, 수학적인 개념을 실제 코드로 작성하며 구현 능력을 키우는데에도 도움이 될 것 입니다.

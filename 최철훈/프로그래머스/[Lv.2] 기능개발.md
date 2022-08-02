# [LV.2] 기능개발

## 서론

알고리즘을 풀이할 때 무작정 코드를 작성해나가는 것 보다 어떤 자료구조를 활용할 수 있는지 생각해보면 문제를 접근하는데에도 큰 도움이 될 수 있습니다. 이번 문제가 앞서 말씀드린 내용을 생각해볼 수 있는 문제라고 생각합니다.



## **문제설명**

프로그래머스 팀에서는 기능 개선 작업을 수행 중입니다. 각 기능은 진도가 100%일 때 서비스에 반영할 수 있습니다.

또, 각 기능의 개발속도는 모두 다르기 때문에 뒤에 있는 기능이 앞에 있는 기능보다 먼저 개발될 수 있고, 이때 뒤에 있는 기능은 앞에 있는 기능이 배포될 때 함께 배포됩니다.

먼저 배포되어야 하는 순서대로 작업의 진도가 적힌 정수 배열 progresses와 각 작업의 개발 속도가 적힌 정수 배열 speeds가 주어질 때 각 배포마다 몇 개의 기능이 배포되는지를 return 하도록 solution 함수를 완성하세요.



## **입출력 예**

![image](https://user-images.githubusercontent.com/48594786/182388632-19a4e72c-0786-4842-a844-cbd19137d790.png)

## 

## 제한사항

- 작업의 개수(progresses, speeds배열의 길이)는 100개 이하입니다.
- 작업 진도는 100 미만의 자연수입니다.
- 작업 속도는 100 이하의 자연수입니다.
- 배포는 하루에 한 번만 할 수 있으며, 하루의 끝에 이루어진다고 가정합니다. 예를 들어 진도율이 95%인 작업의 개발 속도가 하루에 4%라면 배포는 2일 뒤에 이루어집니다. 

## 

## 작성한 코드

```kotlin
import java.util.*

class Solution {
    
    fun solution(progresses: IntArray, speeds: IntArray): IntArray {
        val result = mutableListOf<Int>()
        val queue = LinkedList<Work>()
        
        for(index in progresses.indices) {
            queue.add(Work(progresses[index], speeds[index]))
        }
        
        while(!queue.isEmpty()) {
            var count = 1
            var work = queue.poll()
            var progress = work.progress
            var speed = work.speed
            var rest = 100 - progress
            val diff = getDiff(rest, speed)
            
            while(!queue.isEmpty()) {
                work = queue.peek()
                progress = work.progress
                speed = work.speed
                rest = 100 - progress
                
                if(diff >= getDiff(rest, speed)) {
                    queue.poll()
                    count++
                } else {
                    break
                }
            }
            
            result.add(count)
        }
        
        return result.toIntArray()
    }
    
    private fun getDiff(
        rest: Int,
        speed: Int
    ): Int {
        return if(rest < speed) 1
                else {
                    if(rest % speed == 0) rest / speed
                    else (rest / speed) + 1
                }
    }
    
    class Work (
        val progress: Int,
        val speed: Int
    )

}
```

## 

## 접근방법

1. 이 문제에서의 핵심은 LinkedList 타입으로 선언한 queue 변수입니다.
   
   1-1) 우선 queue에 Work 타입의 클래스에 각 index에 해당하는 progress와 speed값을 추가합니다. 

2. 문제에서 알다시피, n번째의 작업이 다 끝나야지만 그 다음 완료된 기능들의 작업 또한 배포가 가능합니다. 그래서 queue의 특성을 이용해 가장 먼저 들어온 A의 작업을 분석하여 그 다음 작업들이 A작업과 같이 배포될 수 있는지를 확인해야합니다.
   
   2-1) queue에 가장 먼저 들어온 작업을 분석하기 위해서는 queue.poll()을 통해 queue에서 빼내야합니다.
   
   2-2) 이 작업에 저장된 데이터를 활용하여 앞으로 작업을 완료하는데에 며칠이 걸릴지 확인합니다. 코드에는 현재 기준 작업의 속도를 분석하여 며칠후에 작업이 완료되는지 확인할 수 있는 getDiff() 메서드가 있습니다. 이 메서드를 잘 살펴보면 작업 진도와 작업 속도를 비교하여 예외 사항에 따라 값을 반환하는 것을 알 수 있습니다.
   
   2-3) 분석한 작업 데이터를 가지고 queue에 저장된 그 다음 작업을 확인합니다. 이 때에는 poll()이 아닌 peek() 메서드를 통해 queue에서 빼내기 전에 미리 분석합니다. 만약 앞서 분석한 작업의 기능을 배포하는 기간동안 그 다음 작업도 배포가 가능하다면 그때에서야 poll()을 통하여 queue에서 제거합니다. 또한 한 번 기능이 배포될 때 몇개의 작업이 배포되는지를 알아야하기 때문에 카운팅을 해줍니다. 

3. 위의 작업을 queue에 저장된 작업이 텅 비어있을 때까지 반복합니다.

## 

## 결론

이 문제의 핵심은 자료구조 Queue를 활용하여 문제를 해결할 수 있는지의 의도가 엿보이는 (물론 다른 방법으로도 해결 가능할 것 입니다) 문제였습니다. 또한 문제를 해결하는데에 poll(), peek() 과 같은 메서드의의 특성을 잘 알고있는지에 따라서 문제 해결 속도에 영향을 미칠수도 있습니다. 그리고 현재 기준 작업의 속도를 분석하여 며칠후에 작업이 완료되는지에 대해 분기처리 하는 로직과 같이 예외처리도 꼼꼼히해야 문제를 한 번에 해결 할 수 있을 것 입니다.

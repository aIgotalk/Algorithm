# [LV.2] 튜플

## 서론

이 문제를 보자마자 제일 까다로웠다고 생각한건 문자열로 표현된 튜플의 표현 방법이였습니다. 문자열로 표현된 튜플이 List 형식으로 주어졌다면, 문제를 해결하기 훨씬 수월했을 것 같습니다. 그래서 제가 생각하는 이 문제의 의도는 문자열을 잘 다룰 수 있는지를 물어보는 것 같습니다. 제 기준이지만 문제를 이해하고 반환 값을 구성하는 것은 크게 어렵지 않아 보였습니다.



## **문제설명**

셀수있는 수량의 순서있는 열거 또는 어떤 순서를 따르는 요소들의 모음을 튜플(tuple)이라고 합니다. n개의 요소를 가진 튜플을 n-튜플(n-tuple)이라고 하며, 다음과 같이 표현할 수 있습니다.

- (a1, a2, a3, ..., an)

튜플은 다음과 같은 성질을 가지고 있습니다.

1. 중복된 원소가 있을 수 있습니다. ex : (2, 3, 1, 2)
2. 원소에 정해진 순서가 있으며, 원소의 순서가 다르면 서로 다른 튜플입니다. ex : (1, 2, 3) ≠ (1, 3, 2)
3. 튜플의 원소 개수는 유한합니다.

원소의 개수가 n개이고, <u><strong>중복되는 원소가 없는</strong></u> 튜플 `(a1, a2, a3, ..., an)`이 주어질 때(단, a1, a2, ..., an은 자연수), 이는 다음과 같이 집합 기호 '{', '}'를 이용해 표현할 수 있습니다.

- {{a1}, {a1, a2}, {a1, a2, a3}, {a1, a2, a3, a4}, ... {a1, a2, a3, a4, ..., an}}

예를 들어 튜플이 (2, 1, 3, 4)인 경우 이는

- {{2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4}}

와 같이 표현할 수 있습니다. 이때, 집합은 원소의 순서가 바뀌어도 상관없으므로

- {{2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4}}
- {{2, 1, 3, 4}, {2}, {2, 1, 3}, {2, 1}}
- {{1, 2, 3}, {2, 1}, {1, 2, 4, 3}, {2}}

는 모두 같은 튜플 (2, 1, 3, 4)를 나타냅니다.

특정 튜플을 표현하는 집합이 담긴 문자열 s가 매개변수로 주어질 때, s가 표현하는 튜플을 배열에 담아 return 하도록 solution 함수를 완성해주세요.

## **입출력 예**

![image](https://user-images.githubusercontent.com/48594786/182637937-c0cfb8f4-3415-4b57-a3fc-278472aba2ed.png)

## 제한사항

- s의 길이는 5 이상 1,000,000 이하입니다.
- s는 숫자와 '{', '}', ',' 로만 이루어져 있습니다.
- 숫자가 0으로 시작하는 경우는 없습니다.
- s는 항상 중복되는 원소가 없는 튜플을 올바르게 표현하고 있습니다.
- s가 표현하는 튜플의 원소는 1 이상 100,000 이하인 자연수입니다.
- return 하는 배열의 길이가 1 이상 500 이하인 경우만 입력으로 주어집니다.
  
   

## 작성한 코드

```kotlin
class Solution {
    
    fun solution(s: String): IntArray {
        val elementGroupList = mutableListOf<List<Int>>()
        val elementList = mutableListOf<Int>()
        val stringBuilder = StringBuilder()
        for (index in s.indices) {
            val ch = s[index]
            if(ch == ',') {
                if(s[index - 1] == '}' && s[index + 1] == '{') {
                    //집합의 원소가 구분되는 위치의 반점(,)
                    elementList.add(stringBuilder.toString().toInt())
                    stringBuilder.clear()
                    elementGroupList.add(elementList.toList())
                    elementList.clear()
                    continue
                } else {
                    //원소 내 숫자를 구분하는 반점(,)
                    elementList.add(stringBuilder.toString().toInt())
                    stringBuilder.clear()
                }
            } else if (ch >= '0' && ch <= '9') {
                stringBuilder.append(ch)
            }
        }
        
        //저장한 원소가 추가되지 않았다면 한 번 더 저장
        elementList.add(stringBuilder.toString().toInt())
        stringBuilder.clear()  
        elementGroupList.add(elementList.toList())
        elementList.clear()
        
        val checkSet = mutableSetOf<Int>()
        val result = mutableListOf<Int>()
        elementGroupList.sortedBy {
            it.size
        }.forEach { list -> 
            for (index in list.indices) {
                val num = list[index]
                if(checkSet.contains(num).not()) {
                    checkSet.add(num)
                    result.add(num)
                    break
                }
            }
        }
        
        return result.toIntArray()
    }
    
}
```

## 

## 접근방법

1. 우선 입력으로 들어온 s를 List<List<Int>> 타입으로 바뀌는 작업을 먼저 진행하였습니다. 튜플 원소의 사이즈를 기준으로 오름차순 한 후에 문제에 맞게 순차적으로 반환 값을 구성하면 된다고 생각했기 때문입니다. 
   
   1-1) s의 길이만큼 반복문을 돌면서 중요한 분기점을 2가지로 생각했습니다. 첫 번째는, 반점(,)이 나타나는 구간 그리고 두 번째는, 숫자가 나타나는 구간입니다.
   
   1-2) 우선, 반점이 나타나는 구간은 집합의 원소들이 구분되는 지점과 원소 내 숫자를 구분하는 반점으로 구분하였습니다. 집합의 원소는 크기가 최소 1개인 List<Int> 타입으로 구성하였고, 원소들을 저장하는 그룹 리스트는 List<List<Int>> 타입으로 지정하였습니다.
   
   1-3) 숫자가 나오는 구간은 StringBuilder를 활용하여 숫자로 표현된 문자를 덧붙여 주었습니다. 조금 조심해야 할 부분은, 숫자로 표현된 문자가 연속해서 나올 수 있다는 것을 주의해야 합니다.

2. 제가 작성한 알고리즘은 반복문이 끝나고 나서도 한 번 더 해주어야 하는 작업이 있습니다. s의 문자열이 끝나는 지점과 마지막 원소 사이에는 반점(,)으로 구분되지 않고 있습니다. 하지만, 저는 집합의 원소가 구분되는 위치의 반점(,)을 집합의 원소가 구분되는 지점으로 잡았기 때문에 저장한 원소가 있다면 추가하는 작업을 한 번 더 진행합니다.

3. 마지막으로, 반환 값을 구성하려면 문제의 규칙을 잘 이해해야 합니다.
   
   3-1) 우선, 모든 원소가 저장된 elementGroupList를 size를 기준으로 오름차순 정렬합니다. 문제에서 나와있는 튜플이 원소를 구성하는 방법을 보면 {a1}, {a1, a2}, ... {a1, a2, a3, a4, ... , an}이기 때문입니다.
   
   3-1) 자료구조 Set을 활용하여 가장 앞에 위치한 순서대로 튜플 값을 찾아 값을 반환해주면 됩니다.

## 

## 결론

이 문제는 특별한 알고리즘 문제라고 하기보다는 문자열 s를 어떤식으로 활용할지를 빠르게 생각하여야 한다는 것이 중요하게 느껴지는 문제였습니다. 문제 내용은 수학의 정석에서 나올법하게 보여지지만, 실제로 읽어보고 이해해보면 난이도는 크게 어렵지 않은 문제이니 보자마자 겁먹을 문제는 아니라고 생각합니다..! 참고이자 제 생각이지만, 저는 입력값으로 주어진 튜플의 원소들을 표현하는 문자열이 구성된 것을 보고 이것을 어떻게 이용하든지 자료구조 Set은 꼭 필요할 것이라고 생각이 들었습니다. 거의 모든 문제에서 말하고 있는 내용이지만, 효율적이고 간결한 알고리즘을 작성하기 위해서는 문제에 적합한 자료구조를 선택해야 한다는 것 입니다.

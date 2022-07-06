# [LV.1] 행렬의 덧셈

## 서론

행렬의 덧셈문제라 for문으로 간단히 해결할 수 있는 문제이지만, 자바 스트림 연산자처럼 코틀린 표준 확장함수를 이용하여 간단하게 식을 구성하여 해결해보고자 합니다.



## **문제설명**

행렬의 덧셈은 행과 열의 크기가 같은 두 행렬의 같은 행, 같은 열의 값을 서로 더한 결과가 됩니다. 2개의 행렬 arr1과 arr2를 입력받아, 행렬 덧셈의 결과를 반환하는 함수, solution을 완성해주세요.



## **입출력 예**



![image](https://user-images.githubusercontent.com/48594786/177586776-3fc6b1f9-ec20-4105-81bd-ca2e60fb4fc1.png)



## 제한사항

- 행렬 arr1, arr2의 행과 열의 길이는 500을 넘지 않습니다.



### 작성한 코드

```kotlin
class Solution {
    fun solution(arr1: Array<IntArray>, arr2: Array<IntArray>) =
        arr1.mapIndexed { colIndex, colArray ->
            colArray.mapIndexed { rawIndex, rawNum ->
                rawNum + arr2[colIndex][rawIndex]
            }.toIntArray()
        } 
}
```

## 

## 접근방법

1. 제가 여기서 사용한 연산자는 코틀린의 표준 확장함수인 mapIndexed입니다.

2. 우선, map의 네이밍을 가진 연산자는 입력 A를 B로 바꾸어 반환하는 역할을 합니다. Index를 추가로 사용하는 이유는 기준으로 잡은 각 행의 arr1[index]값에 arr2[index]의 값을 더하여 반환하기 위해서 입니다. 조심해야할 점이라면, 여기서의 각 행의 값 또한 IntArray라는 것 입니다.

3. 위의 코드를 예를들어 설명하자면, arr1[index]의 값은 colArray가 될 것 입니다. colArray를 기준으로 다시 한 번 mapIndexed를 사용합니다. colArray의 각 인덱스의 값은 rawNum이 되고, arr2 배열과 같은 좌표에 있는 값은 arr2[colIndex][rawIndex]로 나타낼 수 있습니다. 이 값들을 더해서 각 행과 열의 값을 더한 배열을 반환합니다.

## 

### 결론

사실 2중 for문으로 코드를 구성하여도 코드가 눈에 띄게 줄어든다거나 하는 장점은 없는 문제지만, 코틀린에서 제공하는 표준 확장함수를 이해하고 활용성을 높이기에는 좋은 문제라고 생각합니다. 자바의 스트림 연산자같은 코틀린의 표준 확장함수를 상황에 맞게 잘 활용하는 것은 알고리즘을 문제를 풀이하는데 큰 도움이 될 것 이라 생각합니다.

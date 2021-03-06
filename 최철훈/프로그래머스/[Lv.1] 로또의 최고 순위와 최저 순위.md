# [LV.1] 로또의 최고 순위와 최저 순위

## 서론

이 문제는 로또에 관한 내용이기 때문에, 문제를 읽지 않아도 대부분은 선지식에의해 이해할 수 있는 내용이라 쉽게 풀이할 수 있는 문제입니다. 로또의 순위를 매기는 부분은 거의 동일하며 코틀린의 확장함수를 이용하여 코드도 간결하게 작성할 수 있는 구현문제입니다.



## **문제설명**

`로또 6/45`(이하 '로또'로 표기)는 1부터 45까지의 숫자 중 6개를 찍어서 맞히는 대표적인 복권입니다. 아래는 로또의 순위를 정하는 방식입니다. [1](https://school.programmers.co.kr/learn/courses/30/lessons/77484#fn1)

| 순위    | 당첨 내용        |
| ----- | ------------ |
| 1     | 6개 번호가 모두 일치 |
| 2     | 5개 번호가 일치    |
| 3     | 4개 번호가 일치    |
| 4     | 3개 번호가 일치    |
| 5     | 2개 번호가 일치    |
| 6(낙첨) | 그 외          |

로또를 구매한 민우는 당첨 번호 발표일을 학수고대하고 있었습니다. 하지만, 민우의 동생이 로또에 낙서를 하여, 일부 번호를 알아볼 수 없게 되었습니다. 당첨 번호 발표 후, 민우는 자신이 구매했던 로또로 당첨이 가능했던 최고 순위와 최저 순위를 알아보고 싶어 졌습니다.  
알아볼 수 없는 번호를 `0`으로 표기하기로 하고, 민우가 구매한 로또 번호 6개가 `44, 1, 0, 0, 31 25`라고 가정해보겠습니다. 당첨 번호 6개가 `31, 10, 45, 1, 6, 19`라면, 당첨 가능한 최고 순위와 최저 순위의 한 예는 아래와 같습니다.

| 당첨 번호    | 31                         | 10                           | 45  | 1                         | 6                           | 19  | 결과           |
| -------- | -------------------------- | ---------------------------- | --- | ------------------------- | --------------------------- | --- | ------------ |
| 최고 순위 번호 | <u><strong>31</strong></u> | 0→<u><strong>10</strong></u> | 44  | <u><strong>1</strong></u> | 0→<u><strong>6</strong></u> | 25  | 4개 번호 일치, 3등 |
| 최저 순위 번호 | <u><strong>31</strong></u> | 0→11                         | 44  | <u><strong>1</strong></u> | 0→7                         | 25  | 2개 번호 일치, 5등 |

- 순서와 상관없이, 구매한 로또에 당첨 번호와 일치하는 번호가 있으면 맞힌 걸로 인정됩니다.
- 알아볼 수 없는 두 개의 번호를 각각 10, 6이라고 가정하면 3등에 당첨될 수 있습니다.
  - 3등을 만드는 다른 방법들도 존재합니다. 하지만, 2등 이상으로 만드는 것은 불가능합니다.
- 알아볼 수 없는 두 개의 번호를 각각 11, 7이라고 가정하면 5등에 당첨될 수 있습니다.
  - 5등을 만드는 다른 방법들도 존재합니다. 하지만, 6등(낙첨)으로 만드는 것은 불가능합니다.

민우가 구매한 로또 번호를 담은 배열 lottos, 당첨 번호를 담은 배열 win_nums가 매개변수로 주어집니다. 이때, 당첨 가능한 최고 순위와 최저 순위를 차례대로 배열에 담아서 return 하도록 solution 함수를 완성해주세요.



## **입출력 예**

![image](https://user-images.githubusercontent.com/48594786/178762328-c0b479dc-ad8a-481b-ac75-7c6d7a884a72.png)



## 제한사항

- lottos는 길이 6인 정수 배열입니다.
- lottos의 모든 원소는 0 이상 45 이하인 정수입니다.
  - 0은 알아볼 수 없는 숫자를 의미합니다.
  - 0을 제외한 다른 숫자들은 lottos에 2개 이상 담겨있지 않습니다.
  - lottos의 원소들은 정렬되어 있지 않을 수도 있습니다.
- win_nums은 길이 6인 정수 배열입니다.
- win_nums의 모든 원소는 1 이상 45 이하인 정수입니다.
  - win_nums에는 같은 숫자가 2개 이상 담겨있지 않습니다.
  - win_nums의 원소들은 정렬되어 있지 않을 수도 있습니다.**

 

## 작성한 코드

```kotlin
class Solution {
    
    fun solution(lottos: IntArray, win_nums: IntArray): IntArray =
        intArrayOf(
            7 - lottos.filter {
                win_nums.contains(it) or (it == 0)
            }.size,
            7 - lottos.filter {
                win_nums.contains(it)
            }.size
        ).map {
            if(it <= 6) it
            else 6
        }.toIntArray()
    
}
```

## 

## 접근방법

1. 우선, 문제를 읽어보면 당첨 가능한 최고 순위와 최저 순위를 차례대로 배열에 담아야하기 때문에 반환 값은 2개의 인덱스를 가진 배열이란 것을 알 수 있습니다.

2. 민우가 구매한 당첨 번호는 lottos의 배열입니다. 그러나 여기에는 0을 포함한 숫자들이 들어있습니다. 0이 모두 win_nums에 포함된다고 가정하면 최고 순위를 구할 수 있을 것이고 반대로 win_nums에 모두 포함되지 않는다면 최저 순위를 구할 수 있을 것 입니다.
   
   2-1) 순위를 구하려면 조금 거꾸로 생각해야 합니다. 민우가 구매한 번호가 win_nums에 포함되는 숫자 개수가 6이라고 하면 순위는 1이여야 하기 때문에 7 - 6으로 나타낼 수 있습니다. 이것을 좀더 일반화하여 7 - (0을 포함한 민우가 구매한 로또 번호중 당첨 번호에 해당되는 숫자의 개수)는 최고 순위로 나타낼 수 있습니다.
   
   2.2) 반대로, 최저 순위는 7 - (0을 제외한 민우가 구매한 로또 번호중 당첨 번호에 해당되는 숫자의 개수)로 나타낼 수 있습니다. 어차피 0은 당첨 번호에 포함되지 않기 때문에 특별한 분기처리를 해줄 필요는 없습니다. 

3. 그리고, 2번 설명에 추가적으로 생각해야할 예외적인 케이스가 있습니다. 7 - (로또 번호중 당첨번호에 해당 되는 숫자의 총 개수가 0)인 경우는 위의 일반화된 식으로 계산하면 7등이 되어야합니다. 문제를 읽어보면, 1개 번호가 일치 하는 경우부터 그 이외의 모든 경우는 6순위로 모두 포함시키라고 하였습니다. 그렇기 때문에, map 연산자를 이용하여 6보다 크면 6으로 매핑해 버립니다.

## 

## 결론

이 문제는 선지식에 의해 쉽게 도전해볼 수 있었지만, 그래도 문제를 꼼곰히 읽어야만 예외 케이스를 빠르게 처리할 수 있는 문제였습니다. 여기서 느낀건,, 역시 문제는 꼼꼼히 읽어야 한다는 것 입니다.

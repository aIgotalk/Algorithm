# [LV.2] 오픈채팅방

## 서론

카카오에서 제출하는 알고리즘 문제는 해결하는 과정이 재미있는 것 같습니다. 왜냐하면 문제에 적용가능한 아이디어를 떠올리는게 흥미롭게 다가오고 문제를 보면 볼수록 개선할점이 보이기 때문입니다. 특히나 이번 문제는 좋은 아이디어를 떠올려 코드를 간결하게 작성하는데에 도움이 많이 된 것 같습니다.



## **문제설명**

카카오톡 오픈채팅방에서는 친구가 아닌 사람들과 대화를 할 수 있는데, 본래 닉네임이 아닌 가상의 닉네임을 사용하여 채팅방에 들어갈 수 있다.

신입사원인 김크루는 카카오톡 오픈 채팅방을 개설한 사람을 위해, 다양한 사람들이 들어오고, 나가는 것을 지켜볼 수 있는 관리자창을 만들기로 했다. 채팅방에 누군가 들어오면 다음 메시지가 출력된다.

"[닉네임]님이 들어왔습니다."

채팅방에서 누군가 나가면 다음 메시지가 출력된다.

"[닉네임]님이 나갔습니다."

채팅방에서 닉네임을 변경하는 방법은 다음과 같이 두 가지이다.

- 채팅방을 나간 후, 새로운 닉네임으로 다시 들어간다.
- 채팅방에서 닉네임을 변경한다.

닉네임을 변경할 때는 기존에 채팅방에 출력되어 있던 메시지의 닉네임도 전부 변경된다.

예를 들어, 채팅방에 "Muzi"와 "Prodo"라는 닉네임을 사용하는 사람이 순서대로 들어오면 채팅방에는 다음과 같이 메시지가 출력된다.

"Muzi님이 들어왔습니다."  
"Prodo님이 들어왔습니다."

채팅방에 있던 사람이 나가면 채팅방에는 다음과 같이 메시지가 남는다.

"Muzi님이 들어왔습니다."  
"Prodo님이 들어왔습니다."  
"Muzi님이 나갔습니다."

Muzi가 나간후 다시 들어올 때, Prodo 라는 닉네임으로 들어올 경우 기존에 채팅방에 남아있던 Muzi도 Prodo로 다음과 같이 변경된다.

"Prodo님이 들어왔습니다."  
"Prodo님이 들어왔습니다."  
"Prodo님이 나갔습니다."  
"Prodo님이 들어왔습니다."

채팅방은 중복 닉네임을 허용하기 때문에, 현재 채팅방에는 Prodo라는 닉네임을 사용하는 사람이 두 명이 있다. 이제, 채팅방에 두 번째로 들어왔던 Prodo가 Ryan으로 닉네임을 변경하면 채팅방 메시지는 다음과 같이 변경된다.

"Prodo님이 들어왔습니다."  
"Ryan님이 들어왔습니다."  
"Prodo님이 나갔습니다."  
"Prodo님이 들어왔습니다."

채팅방에 들어오고 나가거나, 닉네임을 변경한 기록이 담긴 문자열 배열 record가 매개변수로 주어질 때, 모든 기록이 처리된 후, 최종적으로 방을 개설한 사람이 보게 되는 메시지를 문자열 배열 형태로 return 하도록 solution 함수를 완성하라.



## **입출력 예**

![image](https://user-images.githubusercontent.com/48594786/182386715-80422e81-5e4e-44e0-acda-dfefb7df2a3c.png)

## 제한사항

- record는 다음과 같은 문자열이 담긴 배열이며, 길이는 `1` 이상 `100,000` 이하이다.
- 다음은 record에 담긴 문자열에 대한 설명이다.
  - 모든 유저는 [유저 아이디]로 구분한다.
  - [유저 아이디] 사용자가 [닉네임]으로 채팅방에 입장 - "Enter [유저 아이디] [닉네임]" (ex. "Enter uid1234 Muzi")
  - [유저 아이디] 사용자가 채팅방에서 퇴장 - "Leave [유저 아이디]" (ex. "Leave uid1234")
  - [유저 아이디] 사용자가 닉네임을 [닉네임]으로 변경 - "Change [유저 아이디] [닉네임]" (ex. "Change uid1234 Muzi")
  - 첫 단어는 Enter, Leave, Change 중 하나이다.
  - 각 단어는 공백으로 구분되어 있으며, 알파벳 대문자, 소문자, 숫자로만 이루어져있다.
  - 유저 아이디와 닉네임은 알파벳 대문자, 소문자를 구별한다.
  - 유저 아이디와 닉네임의 길이는 `1` 이상 `10` 이하이다.
  - 채팅방에서 나간 유저가 닉네임을 변경하는 등 잘못 된 입력은 주어지지 않는다.**

## 

## 작성한 코드

```kotlin
class Solution {

    fun solution(record: Array<String>): Array<String> {
        
        val indexMap = linkedMapOf<Int, Triple<String, String, String>>() //Index, (ID, NickName, Action)
        val userMap = hashMapOf<String, String>()

        for(index in record.indices) {
            val info = record[index].split(" ")

            val action = info[0]
            if(info.size == 3) {
                //enter, change
                val id = info[1]
                val nickName = info[2]

                userMap[id] = nickName

                if(action == "Enter") {
                    indexMap[index] = Triple(id, nickName, action)
                }
            } else {
                //leave
                val id = info[1]
                indexMap[index] = Triple(id, "", action)
            }
        }

        return indexMap.map {
            val nickName = userMap[it.value.first]
            val action = it.value.third
            nickName + if(action == "Enter") "님이 들어왔습니다." else "님이 나갔습니다."
        }.toTypedArray()
    }

}
```

## 

## 접근방법

1. 제가 이 문제풀이를 위해 활용한 자료구조는 Map입니다.
   
   1-1) 근데 제가 선언한 변수를 보면 LinkedMap과 MutableMap이 있습니다.
   
   1-2) LinkedMap을 사용한 이유는 record의 순서대로 (Key, Value) 쌍으로 저장하기 위해서입니다.
   
   1-3) MutableMap을 사용한 이유는 유저의 ID에 대한 최신의 닉네임을 저장하기 위해서입니다.

2. record의 갯수만큼 반복문을 돌립니다.
   
   2-1) record[index]의 값은 변경 기록에 대한 내용인데, 이 값이 포함한 데이터에 따라 Enter, Change 또는 Leave 상태로 나누어 분기처리 해야합니다.
   
   2-2) 반환 값에 포함되어야 할 상태는 Enter와 Leave이기 때문에, 이 값은 indexMap에 각 index를 키로 값을 저장합니다. 저는 값을 저장하는데에 모델을 새로 만든게 아니라 코틀린에서 제공하는 Triple 클래스를 사용하였습니다.
   
   2-3) userMap 변수에 id에 해당하는 닉네임을 저장합니다. Change 상태를 포함하여 가장 최근에 변경 또는 사용한 닉네임을 저장하여 반환 값을 구성할 때 사용하기 위해서입니다.

3. 마지막으로, 값을 반환하기 위해서 indexMap을 사용하여 가공합니다. 왜냐하면, indexMap의 사용 목적은 Enter, Leave인 상태에 대해  값을 순서대로 저장하고 있기 때문입니다.
   
   3-1) 값을 반환할 때는 닉네임이 필요합니다. 하지만 이 때의 닉네임은 각 ID에서 가장 최근에 사용 또는 변경한 닉네임이 필요합니다. 그래서 userMap에서 ID를 기준으로 관리하고 있는 닉네임을 사용합니다.
   
   3-2) Enter인 경우 또는 Leave인 경우 분기 처리하여 메세지를 다르게 구성하여 반환합니다.

## 

## 결론

이 문제를 풀이하다보면 record의 순서대로 어떻게 값을 반환할지가 핵심이라고 할 수 있습니다. 물론 LinkedMap을 활용하지 않아도 충분히 해결할 수 있지만, 순서를 보장하는 Map의 특성을 이해하고 LinkedMap을 활용할 수 있다면 저희가 작성하는 코드의 양이 줄어들어 문제를 빠르게 해결해 나갈 수 있습니다. 이 문제를 풀어보면서 아이디어를 떠올리고 간결한 코드를 작성했을 때 또 한 번에 패스하니 재미있고도 기분이 좋았던 문제입니다.

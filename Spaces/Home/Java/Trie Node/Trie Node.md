
**자바에서 "트라이 노드(Trie Node)"**는 문자열 탐색과 저장에 매우 유용한 **트라이(Trie)** 자료구조를 구성하는 **기본 단위(노드)**

## rie(트라이)란?

트라이는 문자열을 **문자 단위로 트리 형태로 저장**하는 자료구조예요.

- 주로 **문자열 검색, 자동 완성, 접두어 탐색** 등에 사용돼요.
    
- 예: ["cat", "car", "cart"] 이런 단어들을 트라이에 넣으면 공통된 접두어를 하나로 묶어서 저장함.
    

---

## 📦 TrieNode 클래스란?

`Trie` 구조를 구현할 때, 각 노드를 표현하는 클래스가 바로 `TrieNode`입니다.  
이 노드는 다음과 같은 역할을 해요:

```java
class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean isEndOfWord = false;
}
```

|필드|역할|
|---|---|
|`children`|현재 노드에서 이어지는 **다음 문자들을 저장하는 맵**(예: 'a' → 다음 노드, 'b' → 다른 노드)|
|`isEndOfWord`|이 노드가 **어떤 단어의 끝인지 표시**하는 boolean 플래그|

간단한 Trie 사용 예시

```java
class TrieNode {
    Map<Character, TrieNode> children = new HashMap<>();
    boolean isEndOfWord = false;
}

class Trie {
    TrieNode root = new TrieNode();

    public void insert(String word) {
        TrieNode node = root;
        for (char ch : word.toCharArray()) {
            node = node.children.computeIfAbsent(ch, c -> new TrieNode());
        }
        node.isEndOfWord = true;
    }

    public boolean search(String word) {
        TrieNode node = root;
        for (char ch : word.toCharArray()) {
            node = node.children.get(ch);
            if (node == null) return false;
        }
        return node.isEndOfWord;
    }
}
```

## 트라이는 언제 유용할까?

- 수천만 개 단어가 있을 때 **빠르게 검색**하고 싶을 때
    
- 접두어 검색이 많을 때 (`startsWith("ca")`)
    
- 사전(dictionary) 만들기
    
- 자동 완성 기능 구현


```java
(root)
 ├─ '1' ── '1' ── '9' (isEndOfWord = true)
 │                     └─ '5' ── '5' ── '2' ── '4' ── '4' ── '2' ── '1' (isEndOfWord = true)
 └─ '9' ── '7' ── '6' ── '7' ── '4' ── '2' ── '2' ── '3' (isEndOfWord = true)
```


예시
```java
import java.util.*;

class Solution {
    public boolean solution(String[] phone_book) {
        TrieNode root = new TrieNode();

        for (String number : phone_book) {
            if (!insert(root, number)) {
                return false;
            }
        }

        return true;
    }

    // 트라이 삽입 시 접두어 충돌 검사
    private boolean insert(TrieNode root, String word) {
        TrieNode node = root;

        for (char ch : word.toCharArray()) {
            if (node.isEndOfWord) return false; // 다른 번호가 이 번호의 접두어인 경우

            node = node.children.computeIfAbsent(ch, c -> new TrieNode());
        }

        if (!node.children.isEmpty()) return false; // 현재 번호가 다른 번호의 접두어인 경우

        node.isEndOfWord = true;
        return true;
    }

    class TrieNode {
        Map<Character, TrieNode> children = new HashMap<>();
        boolean isEndOfWord = false;
    }
}

```
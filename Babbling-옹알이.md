### 정규식을 사용해 검증하기

| 테스트 15 〉 | 통과 (0.28ms, 33.5MB) |
| --- | --- |
| 테스트 16 〉 | 통과 (0.16ms, 33.5MB) |
| 테스트 17 〉 | 통과 (0.17ms, 33.4MB) |

```jsx
function solution(babbling) {
    var answer = 0;
    for(let i =0; i < babbling.length; i++){
        let word = babbling[i];
        let test = (/^(aya|ye|woo|ma)+$/g).test(word);
        if(test){
            answer++;
        }
    }
    
    return answer;
}
```

**정규식**

처음엔 `/^[aya|ye|woo|ma]+$/g` 대괄호를 사용해 접근했었다. 하지만 소하지만 소괄호와 대괄호의 역할을 혼동했었고, 아래 표를 통해 정리하며 개념을 명확히 이해할 수 있었다.

| 구분 | `()` (소괄호) | `[]` (대괄호) |
| --- | --- | --- |
| 역할 | 그룹화 및 캡처 | 문자 선택 |
| 개별 문자 매칭 | `(abc)` → `"abc"`(전체가 하나의 단위) | `[abc]` → `"a"`, `"b"`, `"c"`(각각 개별 매칭) |

### replace 기능을 활용해 검증하기

| 테스트 15 〉 | 통과 (0.19ms, 33.4MB) |
| --- | --- |
| 테스트 16 〉 | 통과 (0.09ms, 33.4MB) |
| 테스트 17 〉 | 통과 (0.18ms, 33.4MB) |

```jsx
function solution(babbling) {
    var answer = 0;
    const speakables = ["aya", "ye", "woo", "ma"];
    const matchWord = "0";
    
    for(let i =0; i < babbling.length; i++){
        let word = babbling[i];
        speakables.map(speakable => {
            word = word.replace(speakable, matchWord);
        })
        word = word.replaceAll(matchWord, "");
        if(word.length === 0) answer++;
    }
    
    return answer;
}
```

**`matchWord`가 빈 문자열이 아닌 이유**

만약 matchWord가 “” 빈 문자열일 경우, `ex) “myea” → “ma” → true` 로 카운트 되는 예외가 있기 때문이다. 따라서 matchWord를 `speakables`과 상관 없는 문자열로 설정한 후, 최종적으로 모두 대체하여 길이를 판단하도록 처리했다.

### **두 방식의 시간 복잡도 분석**

1. 정규식을 사용해 검증하기
    - `babbling.length = N`인 경우, `for` 루프는 `N`번 반복.
    - 각 단어(`word`)에 대해 **정규식 매칭 (`.test()`) 수행**
        - 정규식은 `|`(OR 연산)과 `+`(반복)을 포함
        - 각 단어의 길이를 `M`이라 할 때, **최악의 경우 O(M)** (정규식 엔진이 문자를 확인)
    
    👉 **결과적으로 `O(N * M)` (N: 단어 개수, M: 단어 길이)**
    
    - 하지만 **정규식 엔진 최적화**로 인해 평균적으로는 **O(N)** 에 가깝게 동작할 가능성이 큼.
2. replace 기능을 사용해 검증하기
    - `for` 루프가 `N`번 반복.
    - 각 단어마다 `speakables` 배열(길이 `4`)을 `map()`으로 순회하면서 `replace()` 수행
        - `replace()`는 최악의 경우 O(M) (문자열 탐색 후 변경)
    - 최악의 경우 **O(4M) ≈ O(M)**
    - `replaceAll()`도 문자열 길이만큼 탐색 → **O(M)**
    
    👉 **총 시간 복잡도: `O(N * M)` (N: 단어 개수, M: 단어 길이)**
    
    - 첫 번째 코드와 동일한 시간 복잡도
    - 그러나 `replace()`는 여러 번 호출되므로 **첫 번째 코드보다 성능이 다소 낮을 수 있음**.

| 코드 | 시간 복잡도 | 최적화 여부 |
| --- | --- | --- |
| `RegExp` 사용 | **O(N * M)** | 비교적 빠름 (정규식 최적화) |
| `replace()` 사용 | **O(N * M)** | `replace()` 반복 호출로 다소 느릴 수 있음 |
### 정규식을 사용해 검증하기

| 테스트 15 〉 | 통과 (0.28ms, 33.5MB) |
| --- | --- |
| 테스트 16 〉 | 통과 (0.16ms, 33.5MB) |
| 테스트 17 〉 | 통과 (0.17ms, 33.4MB) |

```jsx
function solution(babbling) {
    var answer = 0;
    const speakable = ["aya", "ye", "woo", "ma"];
    
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
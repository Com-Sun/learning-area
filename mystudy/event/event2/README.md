### 웹 이벤트를 더 잘 사용하는 방법

웹 이벤트를 사용하는 방법1 문서에서 이벤트 핸들러 프로퍼티를 통해 이벤트를 사용하는 방법을 알아보았다. 이 문서에선 그보다 더 발전된 방법을 다뤄보자.

### 이벤트 핸들러를 추가하고 제거하기
addEventListener() : 이벤트 핸들러를 더하는 현대적인 매커니즘이다.

예시를 통해 확인하자.

[이벤트 핸들러를 추가하고 제거하기](./random-color-addeventlistener.html)

위의 예시에서 이벤트 핸들러를 추가하기 위에 다음과 같은 메소드를 사용했다.

    btn.addEventListener('click', bgChange);

---
이 메소드를 처음보니 어떻게 사용하는지 조금 더 알아보자.

    target.addEventListener(type, listener[, options]);
    target.addEventListener(type, listener[, useCapture]);
    target.addEventListener(type, listener[, useCapture, wantsUntrusted ]); // Gecko/Mozilla only

세 번째 방식은 추천하지 않고, 위의 두 가지 방식만을 알면 될 것 같다. 일반적으로 addEventListener 메소드를 사용하면 첫번째 인자엔 "type"을, 두 번째엔 listener(실행될 코드블럭)을 사용하면 된다

type은 전달되는 이벤트의 타입이다. 클릭, 스크롤, 더블클릭 등 종류가 어마어마하게 많다. 이는 필요할 때 [문서](https://developer.mozilla.org/ko/docs/Web/Events) 에서 확인하자.

---

이 매커니즘은 웹 이벤트를 사용하는 방법 1 문서에서 다룬 방법과 다르게 몇몇 이점을 갖는다. 

1. 이벤트의 추가/제거가 용이하다. -removeEventListener, AbortSignal
2. 같은 리스너에 대해 다수의 핸들러를 등록하는 것이 가능하다.

ex)

1번 매커니즘의 경우 - 불가능

    myElement.onclick = functionA;
    myElement.onclick = functionB;

2번 매커니즘의 경우(인라인) - 사용하지 말것

3번 매커니즘의 경우 - 가능

    myElement.addEventListener('click', functionA);
    myElement.addEventListener('click', functionB);

어떤 매커니즘을 사용해야 하는가?

1번의 경우 오래된 브라우저를 지원할 수 있다. 3번의 경우는 그렇지 않다. 다만 이 단계에선 브라우저 지원 여부는 고민하지 말고, 3번 매커니즘을 주로 연습하자.


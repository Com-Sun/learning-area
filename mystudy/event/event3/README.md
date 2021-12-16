## 웹 이벤트+

이 문서에선 간략하게 이벤트의 관한 몇몇 고급 개념을 다룬다. 완전한 이해를 위한 것이 아니다. 앞으로 마주칠 가능성이 있는 코드 패턴에 대해 익숙해져보자.

### 이벤트 객체

이벤트 핸들러 함수 내부에서 (event) 혹은 (e)와 같은 매개변수가 존재하는 경우가 있다. 이것들은 **이벤트 객체** 라 불리며, 추가적인 정보를 전달하기 위해 이벤트 핸들러에 자동으로 전달된다. 예제를 통해 확인해보자.

[이벤트 객체](./random-color-eventobject.html)

    function bgChange(e) {
    const rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
    e.target.style.backgroundColor = rndCol;
    console.log(e);
    }
    
    btn.addEventListener('click', bgChange);

bgChange(e)라는 메소드 안에 매개변수로 (e)가 존재한다. 여기서 이벤트 객체 e가 이벤트 핸들러(함수 내 코드블럭)에 포함되고, e.target을 통해 범위를 버튼 그 자체로 한정시킨것을 알 수 있다.

note) 이벤트 객체의 target프로퍼티는 항상 이벤트가 발생된 **요소**에 대한 참조이다.

e.target는 다음과 같은 상황에서 매우 유용하다.

**같은 이벤트 핸들러를 다수의 요소에 설정하고, 그것들에 이벤트가 발생했을 때 모두가 같은 행동을 하길 원할 때.**

예제를 통해 확인해보자.

[e.target](useful-eventtarget.html)

### 기본행동 방지하기

Form에서 submit을 누르면 데이터가 제출된다. 하지만 사용자가 양식을 잘못 입력했을 경우 이를 방지해야할 것이다. 이를 어떻게 구현하는지 예시를 통해 알아보자.

[기본행동방지](./preventdefault-validation.html)

위의 예시는 이름을 입력하는 코드이다. Script부분만 간단히 살펴보자.

    const form = document.querySelector('form');
    const fname = document.getElementById('fname');
    const lname = document.getElementById('lname');
    const para = document.querySelector('p');
    
    form.onsubmit = function(e) {
      if (fname.value === '' || lname.value === '') {
        e.preventDefault();
        para.textContent = 'You need to fill in both names!';
      }
    }

form.onsubmit 이벤트 핸들러에 간단한 점검을 구현했다. 만약 성 혹은 이름의 양식이 비었다면 e.preventDefault()를 호출하고, 아래에 메시지를 출력한다. 여기서 e.preventDefault 함수는 처음보니 이해를 위해 문서를 살펴보자.

---

#### e.preventDefault()

Event 인터페이스의 preventDefault() 메서드는 어떤 이벤트를 명시적으로 처리하지 않은 경우, 해당 이벤트에 대한 사용자 에이전트의 기본 동작을 실행하지 않도록 지정한다.

    event.preventDefault();

reference는 [문서](https://developer.mozilla.org/ko/docs/Web/API/Event/preventDefault) 를 확인하자.

---

### 이벤트 버블링과 캡처링

이번 주제는 조금 어렵다. 먼저 개념을 알아보고 예시를 통해 익숙해지자.

**부모 요소를 갖는 요소에서 이벤트(onclick)가 발생했을 때,** 현대의 브라우저는 두 가지 다른 단계(phase)를 실행한다. 각각 캡처링(capturing)과 버블링(bubbling)단계이다. 같은 이벤트 타입의 두 이벤트 핸들러가 한 요소에서 작동되었을 때 무슨 일이 일어나는지를 기술하는 매커니즘이다.

캡처링 : 

* 요소의 가장 바깥쪽의 조상, `<html>` 에서 등록된 onclick 이벤트핸들러가 있는지 검사하고, 그렇다면 실행한다.
* 이후에 다시 `<html>` 내부의 다음 요소로 이동하고, 선택된 요소에 닿을 때까지 반복한다.

버블링 :
* 캡처링과 정확히 반대의 일이 일어난다. 선택된 요소가 onclick 이벤트핸들러를 가졌는지 확인하고, 그렇다면 실행한다.
* 이후에 다음의 조상 요소로 이동하고, `html`요소에 닿을 때까지 계속한다.


    note) 버블링과 캡처링 두 타입의 이벤트 핸들러가 모두 존재하는 경우에 캡처링 단게가 먼저 실행된다.

![](./bubbling-capturing.png)

개념을 숙지했으면 예시를 통해 확인해보자.

[버블링캡처링 오류](./show-video-box.html)

위의 예시는 오류가 있다. 비디오를 클릭했을 때 재생을 하는 동시에 div가 숨겨진다. 다행히도 이를 고칠 방법이 있다.

#### stopPropagation()

이 메소드는 핸들러의 이벤트 객체가 호출되었을 때, 첫번째 핸들러가 실행되지만 이벤트가 더 이상 위로 전파되지 않도록 한다. 이 메소드를 사용하여 오류를 고쳐보자.

      video.onclick = function(e) {
        e.stopPropagation();
        video.play();
      };

### 이벤트 위임

버블링은 이벤트 위임의 이점을 취할수 있다. 자식에게 개별적으로 이벤트리스너를 부착하지 않고, 부모에게 설정한다면 자식에게서 일어난 이벤트가 부모에게까지 올라오는 사실에 의존한다.
 이 문서에선 이러한 개념이 있다 정도로만 알고 넘어가자.
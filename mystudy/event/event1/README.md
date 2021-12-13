### 웹 이벤트를 사용하는 방법


**Note:** 웹 이벤트는 코어JS 언어의 일부가 아니다. 브라우저에 내장된 API의 일부로서 정의된다.

이벤트: 프로그래밍하고있는 시스템에서 일어나는 사건/발생

이벤트 핸들러: 이벤트가 발생하면 실행되는 코드블럭. (이벤트 리스너와 거의 동의어라 볼 수 있다.)

예제를 통해 웹 이벤트를 사용하는 바람직한 방법과 그렇지않은 방법을 비교해보자.

[이벤트 핸들러 프로퍼티](random-color-eventhandlerproperty.html) - 이 방식으로 사용할 것

[인라인 이벤트 핸들러](random-color-eventhandlerattributes.html) - 이 방식으로 사용하지 말 것

이벤트 핸들러 프로퍼티의 경우 HTML과 JS의 분리가 명확하다. 그렇다면 인라인 이벤트 핸들러(이벤트 핸들러 HTML 어트리뷰트)는 어떨까?

    <button onclick="bgChange()">Change color</button>
    //html

    <script>
    function random(number) {
    return Math.floor(Math.random()*number);
    }
    
    function bgChange() {
    const rndCol = 'rgb(' + random(255) + ',' + random(255) + ',' + random(255) + ')';
    document.body.style.backgroundColor = rndCol;
    }
    </script>

html 안에서 onclick="bgChange()"라는 함수를 호출한다. 이 방법은 잘못됐다. 수백개의 btn을 유지보수해야할 경우 끔찍하기 때문이다.

많은 btn을 유지보수할 경우 예시

    const buttons = document.querySelectorAll('button');
    
    for (let i = 0; i < buttons.length; i++) {
    buttons[i].onclick = bgChange;
    }

혹은 forEach문을 통한 예시

    buttons.forEach(function(button) {
    button.onclick = bgChange;
    });
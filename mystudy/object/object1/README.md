### 객체 기본

[oojs](./oojs.html) 를 확인해보자.

    <script>
          const person = {
            name: ['Bob', 'Smith'],
            age: 32,
            gender: 'male',
            interests: ['music', 'skiing'],
            bio: function() {
              alert(this.name[0] + ' ' + this.name[1] + ' is ' + this.age + ' years old. He likes ' + this.interests[0] + ' and ' + this.interests[1] + '.');
            },
            greeting: function() {
              alert('Hi! I\'m ' + this.name[0] + '.');
            }
          };
    </script>

"person" 객체가 보인다. 이중 name, age, gender, interests는 속성, 즉 property이다. 밑의 bio, greeting은 함수, 즉 메소드이다.

이를 통해 객체란 property + method로 이루어진다는 것을 확인할 수 있다. 이런 객체를 **객체 리터럴(object literal)** 이라 부른다. 즉, 객체를 생성할 때 컨텐츠를 그대로 대입하는 것이다. 이는 **클래스**로부터 생성하는 방식과는 다르다. (뒤에서 다시 살펴보자.)


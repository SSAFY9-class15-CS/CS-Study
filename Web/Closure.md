# 클로저

<br/>

# 정의

<aside>
👉🏻 어떠한 함수에서 선언한 변수를 참조하는 내부함수에서만 발생하는 현상

</aside>


<br/>


# 언어별 특징

## javascript

### 실행 컨텍스트

- 실행할 코드에 제공할 환경 정보들을 모아놓은 객체
- 함수가 실행될 때 콜스택에 쌓임
- VariableEnvironment, LexicalEnvironment, ThisBinding

### LexicalEnvironment(정적 환경)

1. environmentRecord
    - 현재 컨텍스트와 관련된 코드의 식별자 정보
    - 컨텍스트를 구성하는 함수에 지정된 매개변수 식별자, 선언한 함수
2. outerEnvironmentReference
    
    현재 호출된 함수가 선언될 당시의 LexicalEnvironment를 참조
    

### 예제 1 (inner의 실행 결과를 return)

```jsx
let outer = function() {
    let a = 1;
    let inner = function() {
        return ++a;
    }
    return inner();
};
let outer2 = outer();
console.log(outer2);
console.log(outer2);
```

![%E1%84~1](https://user-images.githubusercontent.com/85854928/231317851-0f1b9581-708a-462d-b234-933db7157548.JPG)

1. outer 함수에서 변수 a를 선언
2. outer의 내부함수인 inner 함수에서 a의 값을 1만큼 증가시킨 다음 출력
3. inner 함수 내부에서는 a를 선언하지 않았기 때문에 environmentRecord에서 값을 찾지 못하므로 outerEnvironmentReference에 지정된 상위 컨택스트인 outer의 LexicalEnvironment에 접근해서 다시 a를 찾음
4. outer 함수의 실행 컨텍스트가 종료되면 LexicalEnvironment에 저장된 식별자들(a, inner)에 대한 참조를 지움
5. 각 주소에 저장돼 있던 값들은 자신을 참조하는 변수가 하나도 없게 되므로 가비지 컬렉터의 수집 대상이 됨

### 예제 2 (inner 함수를 return)

```jsx
let outer = function() {
    let a = 1;
    let inner = function() {
        return ++a;
    }
    return inner;
};
let outer2 = outer();
console.log(outer2());
console.log(outer2());
```

![%E1%84~2](https://user-images.githubusercontent.com/85854928/231317896-7fb74864-f6d1-4378-a773-e7fa435ef461.JPG)

![%E1%84~3](https://user-images.githubusercontent.com/85854928/231317920-e2e4696b-8a5b-4698-a995-cba5c470717d.JPG)

1. outer 함수의 실행 컨텍스트가 종료될 때 outer2 변수는 outer의 실행 결과인 inner 함수를 참조
2. outer2를 호출하면 반환된 함수인 inner가 실행
3. inner 함수의 outerEnvironmentReference에는 inner 함수가 선언된 위치의 LexicalEnvironment가 참조복사되는데, inner 함수는 outer 함수 내부에서 선언됐으므로, outer 함수의 LexicalEnvironment가 담김
4. outer에서 선언한 변수 a에 접근해서 1만큼 증가 시킨 후 그 값인 2를 반환하고, inner 함수의 실행 컨텍스트가 종료

### inner 함수의 실행 시점에 outer 함수는 이미 실행이 종료된 상태인데 어떻게 outer 함수의 LexicalEnvironment에 접근할 수 있을까?

- 어떤 값을 참조하는 변수가 하나라도 있다면 그 값은 수집대상에 포함시키지 않는 가비지 컬렉터의 동작방식 때문
1. outer 함수는 실행 종료 시점에 inner 함수를 반환 
2. 외부 함수인 outer의 실행이 종료되더라도 내부 함수인 inner 함수는 언젠가 outer2를 실행함으로써 호출될 가능성이 생김
3. 언젠가 inner 함수의 실행 컨텍스트가 활성화되면 outerEnvironmentReference가 outer 함수의 LexicalEnvironment를 필요로 할 것이므로 수집 대상에서 제외됨
4. 그래서 inner 함수가 a 변수에 접근할수 있는 것

---

## java

### funtional interface

1. Function<T, R>
    
    1개의 파라미터를 받아서 1개의 결과를 반환
    

```java
Function<String, Integer> stringtoInteger = string -> Integer.parseInt(string);
```

1. Supplier<T>
    
    값을 생성
    

```java
int number = 5;
Supplier<Integer> supplier = () -> number * number;
System.out.println(supplier.get());
```

### 예제

```java
public static class Closure {
    private Supplier<Integer> add10(int num) {
        return () -> num + 10;
    }
}

public static void main(String[] args) throws IOException {
    Closure closure = new Closure();
    Supplier<Integer> call = closure.add10(10);
    System.out.println(call.get());
}
```

```java
public class ClosureTest {
	private Integer b = 2;
    
	private Stream<Integer> calculate(Stream<Integer> stream, Integer a) {
	    return stream.map(t -> t + a + b);
	}
	
	public static void main(String[] args) {
	    List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
	    List<Integer> result = new ClosureTest()
	        .calculate(list.stream(), 3)
	        .collect(Collectors.toList());
	    System.out.println(result);
	}
}

```

- java에는 LexicalEnvironment가 존재하지 않음
- java는 매개변수를 전달할 때 call by value로 값을 복사해서 사용함 (객체가 전달될 때는 참조값을 담은 레퍼런스를 복제하기 때문에 객체는 공유될 수 있음)
- 클로저 함수에서 사용되는 변수가 복제되기 때문에 외부에서 함수를 호출해도 생성되었을 때 전달받은 변수값을 사용할 수 있음
- 외부변수가 내부함수에서 변경될 수 없기 때문에 외부변수를 변경하려 하면 컴파일 에러가 발생함
    - final 키워드를 지정해서 사용하는 것을 권장함

# 메모리 관리

<aside>
👉🏻 메모리 소모는 클로저의 본질적인 특성이지만 메모리 누수의 위험이 있음

</aside>

## 관리방법

- 클로저는 필요에 의해 의도적으로 함수의 지역변수를 사용해서 메모리를 소모함으로 발생함
- 필요성이 사라진 시점에 더는 메모리를 소모하지 않게 해주면 됨
- 참조카운트를 0으로 만드는 방법
    - 식별자에 참조형이 아닌 기본형 데이터(javascript에서는 null이나 undefined)를 할당

# 활용 사례

## 콜백함수 내부에서 외부 데이터를 사용하고자 할 때

```jsx
let fruits = ["apple", "banana", "grape"];
let $ul = document.createElement("ul");

fruits.forEach(function(fruit) {
	let $li = document.createElement("ul");
	$li.innerText = fruit;
	$li.addEventListener("click", function() {
		alert(fruit);
	});
	$ul.appendChild($li);
});
```

- $l의 addEventListener에 넘겨준 콜백함수가 외부함수의 매개변수인 fruit를 참조하고 있음

## 접근 권한 제거(정보 은닉)

```jsx
let car = {
    fuel : 100,
    move : 0,
    run: function() {
        let km = Math.ceil(Math.random() * 6);
        this.fuel -= km;
        this.move += km;
        console.log(km + "km 이동, 총 " + this.move + "km");
    }
}
```

```jsx
let createCar = function() {
    let fuel = 100;
    let move = 0;
    return {
        get move() {
            return move;
        },
        run: function() {
            let km = Math.ceil(Math.random() * 6);
                    fuel -= km;
                    move += km;
                    console.log(km + "km 이동, 총 " + move + "km");
        }
    }
};

let car = createCar();
console.log(car.fuel);
console.log(car.move);
car.run();
```

- 정보 은닉은 어떤 모듈의 내부 로직에 대해 외부로의 노출을 최소화해서 모듈간의 결합도를 낮추고 유연성을 높이고자 하는 것
- 객체로 만든 car는 외부에서 fuel, move를 직접 수정할 수 있음
- 함수로 만든 createCar에서는 fuel, move를 직접 수정할 수 없고, return해준 move의 getter 함수와 run 함수만 사용할 수 있음

## 부분 적용 함수

```jsx
let add = function () {
    let result = 0;
    for (let i = 0; i < arguments.length; i++) {
        result += arguments[i];
    }
    return result;
};

let addPartial = add.bind(null, 1, 2, 3, 4, 5);
console.log(addPartial(6, 7, 8, 9, 10));
```

```jsx
let partial = function() {
    let originalPartialArgs = arguments;
    let func = originalPartialArgs[0];
    
    return function() {
        let partialArgs = Array.prototype.slice.call(originalPartialArgs, 1);
        let restArgs = Array.prototype.slice.call(arguments);
        return func.apply(this, partialArgs.concat(restArgs));
    };
};

let add = function () {
    let result = 0;
    for (let i = 0; i < arguments.length; i++) {
        result += arguments[i];
    }
    return result;
};

let addPartial = partial(add, 1, 2, 3, 4, 5);
console.log(addPartial(6, 7, 8, 9, 10));
```

- 부분 적용 함수는 n개의 인자를 받는 함수에 미리 m개의 인자만 넘겨 기억시켜두고, 나중에 n - m개의 인자를 넘긴 후 실행 결과를 얻을 수 있는 함수
- 첫 번째 인자에 원본 함수를, 두 번째 인자부터는 미리 적용할 인자들을 전달함
- 반환할 함수에서 나머지 인자들을 받고, 이를 모아 원본 함수를 호출함

## 커링 함수

```jsx
let curry = function(func) {
    return function(a) {
        return function(b) {
            return func(a, b);
        }
    }
}

let getMax = curry(Math.max)(10);
console.log(getMax(5));
console.log(getMax(15));
```

```jsx
let curry = func => a => b => func(a, b);
```

- 커링 함수는 여러 개의 인자를 받는 함수를 하나의 인자만 받는 함수로 나눠서 순차적으로 호출될 수 있게 체인 형태로 구성한 것
- 각 단계에서 받은 인자들을 모두 마지막 단계에서 참조하기 때문에 GC되지 않고, 마지막 호출로 실행 컨텍스트가 종료된 후에 한꺼번에 GC의 수거대상이 됨.

## 참고

코어 자바스크립트 - 정재남, 위키북스

[https://www.youtube.com/watch?v=PVYjfrgZhtU](https://www.youtube.com/watch?v=PVYjfrgZhtU)

[https://www.youtube.com/watch?v=EWfujNzSUmw](https://www.youtube.com/watch?v=EWfujNzSUmw)

[https://www.youtube.com/watch?v=PJjPVfQO61o](https://www.youtube.com/watch?v=PJjPVfQO61o)

[https://sabarada.tistory.com/82](https://sabarada.tistory.com/82)

[https://incheol-jung.gitbook.io/docs/q-and-a/java/undefined](https://incheol-jung.gitbook.io/docs/q-and-a/java/undefined)

[https://taes-k.github.io/2021/08/16/java-lambda-closure/](https://taes-k.github.io/2021/08/16/java-lambda-closure/)

[https://velog.io/@janeljs/Java-Closure](https://velog.io/@janeljs/Java-Closure)

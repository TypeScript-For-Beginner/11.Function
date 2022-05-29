# 11.Function
조민경 11. 함수

<aside>
💡 이번 장에서는 함수에서 JavaScript와 TypeScript 문법을 비교해보며, 타입을 어떻게 설정하는지 알아보도록 하자.

</aside>

## 11.1 함수의 인자

JavaScript에서는 함수에 매개변수를 적고 인자를 넘겨주지 않으면 undefined로 값을 리턴해주지만, TypeScript에서는 함수에 매개변수를 적고 인자를 넘겨주지 않으면 Error가 발생한다. 이는 TypeScript에서 매개변수를 선언하면 ‘**필수적으로 인자를 입력해야 한다(필수값)**’는 것을 의미한다.

<aside>
💡 **필수값이란?** 함수의 매개변수를 선언하면  null 혹은 undefined라도 인자로 넣어야 한다는 뜻이다. 컴파일러는 정의한 매개변수 값이 넘어왔는지 확인한다. 더불어 정의된 매개변수 값만 받을 수 있고 추가로 인자를 받을 수 없다.

</aside>

## 11.2 함수의 기본 타입 선언

JavaScript의 문법으로 함수를 작성할 때는 예제 11-1처럼 작성한다.

> 예제 11-1
> 

```jsx
function sum(number1, number2){
	return number1 + number2;
	console.log(number1+number2);
}

console.log(sum(10,20)); //result: 30
console.log(sum(10,20,30)); //result: 30
console.log(sum(10)); //result: NaN
```

TypeScript는 JavaScript와는 다르게 각 **매개변수 뒤에** `:타입`을 입력해주고 **괄호 뒤에는 함수가 반환하는** `:타입`을 입력한다. 반환하는 타입이 없는 경우 `:void`로 입력한다.

타입 종류는 **Boolean**(불리언), **Number**(숫자), **String**(문자열), **Array(**배열) , **유니언 타입**(다중타입: 문자열과 숫자를 동시에 가지는 배열), **any**(타입을 단언할 수 없을 때)가 있다. 

TypeScript에서는 어떤 방식으로 작성되는지 아래의 예제를 통해 알아보자.

### 11.2.1 반환 값이 있는 경우

함수의 반환 값이 있는 경우 반환하는 자료형의 타입을 기재한다.

> 예제 11-2
> 

```tsx
function sum(number1: number, number2: number): number{
	return number1 + number2;
}
console.log(sum(10,20)); //result: 30
console.log(sum(10,20,30)); //Error!(파라미터 개수가 많아서 에러)
console.log(sum(10)); //Error!(파라미터 개수가 너무 적어서 에러)
```

> 예제 11-3
> 

```tsx
function isChildren(age: number): boolean{
	return age < 19;
}
console.log(isChildren(47)); //result: false
console.log(isChildren(5)); //result: true
```

### 11.2.2 반환 값이 없는 경우 `void`

반환 값이 없는 경우 void를 기재한다.

> 예제 11-4
> 

```tsx
function sum(number1:number, number2:number): void{
	console.log(number1 + number2);
}
```

## 11.3 선택적 매개변수 ‘?’

JavaScript에서는 예제 11-5와 같이 매개변수의 개수만큼 인자를 넘기지 않아도 된다. 하지만 함수의 기본 타입을 선언하게 되면 JavaScript의 특성과는 다르게 Error가 발생한다. 예제 11-5는 JavaScript의 문법으로 작성한 예시이다.

> 예제 11-5
> 

```jsx
function morning(name){
	return `Good morning ${name || 'everyone'}`;
}

console.log(morning()); //result: Good morning everyone
console.log(morning('mjo')); //result: Good morning mjo
console.log(morning(123)); //result: Good morning 123
```

결과를 확인하면 JavaScript로 작성한 코드는 별도의 Error가 발생하지 않는다.

TypeScript에서 위와 같은 JavaScript의 특성을 살리기 위해 **선택적 매개변수**가 등장했다. 변수명 앞에 ‘?’를 붙이면 그 매개변수는 Optional한 값이 되고  Optional한 값은 인자를 넘기지 않아도 에러가 발생하지 않는다.

> 예제 11-6
> 

```tsx
function morning(name?: string): string {
	return `Good morning ${name || 'everyone'}`;
}

console.log(morning()); //result: Good morning everyone
console.log(morning('mjo')); //result: Good morning mjo
console.log(morning(123)); //Error!(타입에러)
```

TypeScript의 매개변수 타입 확인 절차는 거치면서 JavaScript의 특성은 살리는 모습을 확인할 수 있다.

여기서 선택적 매개변수를 사용할 때 주의할 점이 있는데, **선택적 매개변수가 필수 매개변수보다 앞에 위치하면 에러가 발생**한다는 사실이다. 만약 에러 없이 선택적 매개변수를 앞에 위치하고 싶다면 ‘?’를 제거하고 ‘| undefined’를 선언하면 된다. 예제 11-7이 그 예시이다.

> 예제 11-7
> 

```tsx
function morning(name: string | undefined, time: number): string {
	return `Good morning ${name || 'everyone'}. Time is ${time || 8}.`;
}

console.log(morning()); //Error!
console.log(morning('mjo', 7)); //result: Good morning mjo. Time is 8.
console.log(morning(undefined, 123)); //result: Good morning everyone. Time is 123.
```

## 11.4 매개변수 초기화

타입을 적어주는 것과 다른 방법으로 **매개 변수를 선언할 때 값을 미리 초기화** 할 수 있다. 필수 매개변수 앞에 위치해도 정상 작동을 하며 따로 타입을 지정해주지 않아도 된다.

> 예제 11-8
> 

```jsx
function sum(a: number, b = 2022): number {
	return a+b;
}
console.log(sum(10,undefined)); //result: 2032
console.log(sum(10,20,30)); //Error!(파라미터 개수가 많아서 에러)
console.log(sum(10)); //result: 2032
```

## 11.5 REST 문법이 적용된 매개변수

**전개문법**으로 받은 매개변수는 **배열**이기 때문에 타입을 배열로 받는다.

> 예제 11-9
> 

```jsx
function sum(...numbers: number[]): number{
	return numbers.reduce((result, number) => result + number, 0);
}

console.log(sum(10, 20, 30)); //reeult: 60
console.log(sum(1, 2, 3, 4, 5, 6, 7, 8, 9, 10)); //result: 55
```

## 11.6 this

this의 타입을 정할 때는 함수의 첫 번째 매개변수 자리에 `this`를 쓰고 타입을 입력한다.

> 예제 11-10
> 

```tsx
function 함수명(this: 타입) {
  // ...
}
```

매개변수와 같이 this의 타입을 적어주지만 실제로 인자값을 받는 매개변수는 `this: 타입`**을 제외한 나머지**임으로 헷갈리면 안된다.

> 예제 11-11
> 

```tsx
interface User {
  name: string,
  age: number,
  init(this: User): () => {};
}

let user1: User = {
  name: 'mjo',
  age: 20,
  init: function(this: User) {
    return () => {
      return this.age;
    }
  }
}

let getAge = user1.init();
let age = getAge();
console.log(age); // return: 20
```

## 11.7 **콜백에서의 this**

콜백 함수에서 `this`는 콜백으로 함수가 전달되었을 때 `this`를 구분해줘야 할 때가 있다. 

> 예제 11-12
> 

```tsx
interface BrowserEL {
  addClickListener(onclick: (this: void, e: Event) => void): void;
}

class Handler {
    info: string;
    onClick(this: Handler, e: Event) { 
		//BrowserEL의 this타입은 void인데 Handler라고 타입선언하여 에러
        this.info = e.message;
    }
}
let handler = new Handler();
browserEL.addClickListener(handler.onClick); //Error!
```

만약 `BrowserEL` 인터페이스에 맞춰 `Handler`를 구현하려면 아래와 같이 변경해야한다.

> 예제 11-13
> 

```tsx
class Handler {
    info: string;
    onClick(this: void, e: Event) {
    //this의 타입이 void이기 때문에 여기서 this.변수명 사용 불가
        console.log('clicked!');
    }
}
let handler = new Handler();
browserEL.addClickListener(handler.onClick);

```

## 11.8 함수 오버로드

JavaScript는 동일한 매개변수지만 타입을 다르게 받을 수 있다. TypeScript에서 그 특징을 구현하기 위해 함수 위에 매개변수의 다른 타입을 적어둘 수 있다. 전달받은 매개변수의 개수나 타입에 따라 다른 동작을 하게하는 것을 오버로드라 한다.

### 11.8.1 매개변수의 개수는 동일하지만, 타입이 다른 경우

첫 번째 함수의 매개변수는 모두 `string`타입을 가지며, 두 번째 함수는 `number`타입의 매개변수를 가진다. 즉, 반환하는 값이 `string`일 수도 `number`일 수도 있기 때문에 반환되는 자료형으로는 `any`를 선언한다.

> 예제 11-14
> 

```tsx
function sum(a: string, b: string): string;
function sum(a: number, b: number): number;

function sum(a: any, b: any): any{
    return a + b;
}

console.log(sum(1, 2)); //result: 3
```

### 11.8.2 매개변수의 개수는 다르지만, 타입은 같은 경우

세 개의 함수 선언이 되어 있으며 각 함수는 타입은 같지만 인자를 받는 매개변수의 개수가 다르다. 첫 번째 함수는 매개변수가 1개, 두 번째 변수는 매개변수가 2개, 세 번째 변수는 매개변수가 3개이다.

매개변수의 개수가 다르기 때문에 선택적 매개변수 ‘?’를 사용하며, 반환 값은 ‘||’를 사용하여 null 또는 undefined인 경우 0을 반환한다.

> 예제 11-15
> 

```tsx
function sum(a: number) : number;
function sum(a: number, b: number): number;
function sum(a: number, b: number, c: number): number;

function sum(a: number, b?: number, c?: number): number{
    return a + (b || 0) + (c || 0);
}

console.log(sum(1,2,3)); //result: 6
```

### 11.8.3 매개변수의 개수와 타입이 다른 경우

> 예제 11-16
> 

```tsx
interface NicknameMaker{
	name: string,
	num: number,
    init(this: NicknameMaker): () =>{};
}

function makeNickname(name: string, num: number | string): NicknameMaker | string {
	if(typeof num === "number"){
		return {
			name,
			num,
            init: function(this: NicknameMaker){
                return () => {
                    return this.name+this.num;
                }
            }
		};
	}else{
		return "이름 다음에는 숫자로 입력해주세요.";
	}
}

const getNickName= makeNickname("Mjo", 303).init(); //Error!
console.log(getNickName()); 
```

예제 11-16처럼 작성하면 값을 판별하기가 어렵다. 때문에 예제 11-17과 같이 작성해야 한다.

> 예제 11-17
> 

```tsx
interface NicknameMaker{
	name: string,
	num: number,
    init(this: NicknameMaker): () =>{};
}

function makeNickname(name: string, num: number): NicknameMaker;
function makeNickname(name: string, num: string): string;
function makeNickname(name: string, num: number | string): NicknameMaker | string {
	if(typeof num === "number"){
		return {
			name,
			num,
      init: function(this: NicknameMaker){
          return () => {
              return this.name + this.num;
          }
      }
		};
	}else{
		return "이름 다음에는 숫자로 입력해주세요.";
	}
}

const getNickName = makeNickname("Mjo", 303).init();
console.log(getNickName()); // result: Mjo303
```

# 타입스크립트 
- 타입이 없는 자바스크립트를 보완하기 위해 나옴
- 자바스크립트의 묵시적 형변환을 보완해줌
- 타입이 있기에 프로그램 유지보수 시 편해짐
- 매개변수가 잘못들어가거나 매개변수에 올바른 타입이 들어가지않으면 에러를 발생시킴 , 리턴타입도 존재


# 타입스크립트 타입 선언

~~~ ts
// 문자열 숫자, 배열 타입

let str:string="hello"
let num:number=10
let arr:Array<number>=[1,2,3]
let items:number[]=[1,2,3]
let heros:Array<string>=['Capt','Thor']


// 튜플
let address:[string,number]=["hi",123]

//객체
let obj:object={}
let person: {name:string,age:number}={
    name:'thor'
    age:100
}

// 불리언
let show:boolean=true

// any (아무거나)
let todoitems:any

~~~

# 타입스크립트 함수 타입 지정

~~~ts
// 함수의 파라미터에 타입을 정의하는 방식
function sum(a: number, b: number) {
    return a+b
}
// 함수의 반환 값에 타입을 정의하는 방식
function add(): number{ // 리턴
    return 10
}

// 함수에 타입을 정의하는 방식 (가장 잘 쓰는 방식)
function sub(a: number, b: number): number {
    return a-b
    
}

sub(1,2,3,4)  // 함수의 매개변수와 맞지않아 사용불가
// => 타입스크립트에서는 이렇게 사용 불가 (정해진 parameter자체를 인식함)
~~~


# 타입스크립트  옵셔널 파라미터 
- ?를 붙이므로 인해 parameter가 들어올수도있고 안들어올수도있다는것을 명시가능 


~~~ ts
const player:{name:string, age?:number}={name:"nico"}

// b에 string이 들어올수있고 안들어올수있음 
function log(a: string, b?: string) {
    
} 

	//함수의 void 반환값이 없다는것을 명시
function log2(a: string, b?: string):void {
    
} 

~~~ 
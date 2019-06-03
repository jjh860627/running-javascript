# Chapter9. 객체와 객체지향 프로그래밍

## 9.1 프로퍼티 나열
-  객체의 프로퍼티 나열은 배열과 달리 순서가 보장 되지 않음
	### 9.1.1 for...in
	- 객체 프로퍼티 나열은 for ... in을 사용 (Iterable 객체는 for ... of 사용)
	- for ... in 루프에는 키가 심볼인 프로퍼티는 포함되지 않음
	- 예외상황을 피하기 위해 hasOwnProperty를 체크하는게 좋음
 	### 9.1.2 Object.keys()
	- 객체에서 나열 가능한 문자열 프로퍼티를 배열로 반환
	- hasOwnProperty를 체크할 필요는 없음.
 ## 9.2 객체지향 프로그래밍
- 데이터와 기능을 논리적으로 묶어 놓은 패러다임
 	### 9.2.1 클래스와 인스턴스 생성
 	- 클래스의 생성자는 constructor(){} 와 같이 정의
 	- 해당 클래스의 인스턴스를 만들 때는 "new" 키워드 사용
 	- "instanceof" 연산자를 이용해 객체가 클래스의 인스턴스인지 확인 가능(java와 동일)

 	~~~javascript
 	class Car{
 		//생성자
 		constructor(make, model){
 			this.make = make;
 			this.model = model;
 			this.userGears = ['P', 'N', 'R', 'D'];
 			this.userGear = this.userGears[0];
 		}
 		//메소드에는 function 키워드를 넣지 않음.
 		shift(gear){
 			if(this.userGears.indexOf(gear) < 0){
 				throw new Error(`Invalid gear: ${gear}`);
 			}
 			this.userGear = gear;
 		}
	}
	~~~
	
	- javascript에는 접근제어자가 없음
	- 프로퍼티를 보호해야 한다면 스코프를 이용하고 WeakMap 인스턴스를 사용
	~~~javascript
	const Car = (function(){
		const carProps = new WeakMap(); //WeakMap 사용
		const userGears = ['P', 'N', 'R', 'D'];
		class Car{
			constructor(make, model){
	 			this.make = make;
	 			this.model = model;
	 			carProps.set(this, {userGear : userGears[0]});
 			}
 			get userGear(){ return carProps.get(this).userGear;}
 			set userGear(value){
 				if(userGears.indexOf(value) < 0){
 					throw new Error(`Invalid gear: ${value}`);
 				}
 				carProps.get(this).userGear = value;
 			}
		}
		return Car;
	})();
	
	### 9.2.2 클래스는 함수다
	- ES5 에서는 Car 클래스를 아래와 같이 생성
	~~~javascript
	function Car(make, model){
		this.make = make;
		this.model = model;
		this.userGears = ['P', 'N', 'R', 'D'];
		this.userGear = this.userGears[0];
	}
	- ES6의 클래스의 경우도 typeof 연산자의 결과가 function임(하지만 class의 경우는 new 키워드를 반드시 사용해야함)
	~~~	
	### 9.2.3 프로토타입
	- 클래스의 인스턴스에서 사용할 수 있는 메서드 = 프로토타입 메서드
	- 프로토타입 메서드는 Car.prototype.shift 처럼 표기
	- 클래스는 Car 처럼 항상 첫글자를 대문자로 표기하는게 일반적(파스칼 표기법)
	- 동적 디스패치 : 객체의 프로퍼티나 메서드에 접근하려고  할 때 해당 객체에 존재 하지 않으면 객체의 프로토타입에서 해당 프로퍼티나 메서드를 찾고 프로토타입에서 찾지 못하면 프로토타입의 프로토타입까지 찾음(부모)
	- 인스턴스에서 프로토타입에 존재하는 메서드나 프로퍼티를 정의하면 프로토타입에 있는 것을 가리는 효과가 나타남

	### 9.2.4 정적 메서드
	- static 키워드를 사용한 메서드
	- 특정 인스턴스에 적용되지 않고 클래스명.xxx() 와 같이 호출하는게 좋음.
	
	~~~javascript
	class Car{
		static getNextVin(){
			return Car.nextVin++; //this.nextVin이라고 써도 되지만 정적 메서드임을 상기하기 위해 Car를 씀
		}
		constructor(make, model){
			this.make = make;
			this.model = model;
			this.vin = Car.getNextVin();
		}
	}
	
	Car.nextVin = 0;
	
	const car1 = new Car("Tesla", "S");
	const car2 = new Car("Mazda", "3");
	const car3 = new Car("Mazda", "3");
	
	
	console.log(car1.vin);
	console.log(car2.vin);
	console.log(car3.vin);
	~~~	
 
 	### 9.2.4 상속
 	- 클래스의 인스턴스는 클래스의 기능을 모두 상속
 	- 자바스크립트는 프로토타입 체인(객체 => 객체의 프로토타입 => 프로토타입의 프로토타입(상속된 부모)으로 메서드나 프로퍼티를 찾음
 	
 	
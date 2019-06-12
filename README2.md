# Chapter12.이터레이터와 제너레이터
ES6에 처음 도입된 개념으로 이터레이터는 제너레이터에 의존하는 개념
## 12.1 이너레이션 프로토콜
- 이터레이터 프로토콜은 모든 객체를 이터러블 객체로 바꿀 수 있음
- 이터레이션 프로토콜 조건
   - 심볼메소드 Symbol.iterator가 존재
   - Symbol.iterator 메소드는 value와 done 프로퍼티가 있는 객체를 반환하는 next() 메서드가 존재


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
	~~~
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
 
 	### 9.2.5 상속
 	- 클래스의 인스턴스는 클래스의 기능을 모두 상속
 	- 자바스크립트는 프로토타입 체인(객체 => 객체의 프로토타입 => 프로토타입의 프로토타입(상속된 부모)으로 메서드나 프로퍼티를 찾음
 	- extends 키워드를 사용해 클래스를 상속받음.
 	- 반드시 생성자에서 상속받은 부모클래스의 생성자(super())를 호출해 줘야함.(자바의 경우는 default생성자의 경우는 생략가능)
 	
 	### 9.2.6 다형성
 	- 객체지향 언어에서 여러 슈퍼클래스의 멤버인 인스턴스의 성질을 말함
 	- instanceof 연산자로 객체의 클래스 확인
 	- 자바스크립트의 모든 객체는 Object 클래스의 서브클래스임.

 	### 9.2.7 객체 프로퍼티 나열 다시보기
 	- obj.hasOwnProperty(x)는 obj에 프로퍼티 x가 있다면 true를 반환, 정의되지 않았거나 프로토타입 체인에만 정의된 경우는 false 반환
 	- 프로퍼티를 프로토타입에 정의하지 못하도록 강제하는 장치가 없으므로 항상 "hasOwnProperty"를 사용하는 편이 좋음.
 	- 슈퍼클래스 생성장에서 선언한 프로퍼티는 서브클래스 인스턴스에도 정의됨(프로토타입에 정의되는것이 아님)
 	
 	~~~javascript
 		class Super{
 			constructor(){
 				this.name = 'Super';
 				this.isSuper = true;
 			}
		}
		//클래스의 프로토타입에 sneaky 프로퍼티 할당
		Super.prototype.sneaky = 'not recommended'; //권장하지 않음
		
		class Sub extends Super{
			constructor(){
				super();
				this.name = 'Sub';
				this.isSub = true;
			}
		}
		
		const obj = new Sub();
		
		for(let p in obj){
			console.log(`${p}: ${obj[p]}` + (obj.hasOwnProperty(p) ? '' : '(inherited)'));
		}
	~~~
	
	### 9.2.8 문자열 표현
	- Object 클래스의 toString() 정의하여 사용
	
	### 9.3 다중상속, 믹스인, 인터페이스
	- 자바스크립트에는 다중상속을 지원하지 않으며 인터페이스 또한 없음.
	- 자바스크립트가 다중 상속이 필요한 문제에 대한 해답으로 내놓은 개념이 믹스인
	~~~javascript
		class InsuarancePolicy{}
		const ADD_POLICY = Symbol();
		const GET_POLICY = Symbol();
		const IS_INSURED = Symbol();
		const _POLICY = Symbol();
		
		function makeInsurable(o){
			o[ADD_POLICY] = function(p){ this[_POLICY] = p; }
			o[GET_POLICY] = function(){ return this[_POLICY]; }
			o[IS_INSURED] = function(){ return !!this[_POLICY]; }
		}
		
		makeInsurable(Car.prototype); //Car의 인스턴스가 아닌 prototype 자체를 넘김으로써 모든 Car Instance에 일괄적용
	~~~
 	
# Chapter10. 맵과 셋

## 10.1 맵
 - set(key, value) : 맵에 값 추가, 같은 키를 다시 set하는 경우 value가 교체됨
 - get(key) : 맵에 저장된 해당 키값에 해당하는 value을 가져옴
 - has(key) : 맵에 키가 존재하는지 확인
 - size 프로퍼티 : 맵의 사이즈 
 - keys() : 맵의 키를 Iterable로 반환
 - values() : 맵의 value를 Iterable로 반환
 - entirs() : 맵의 key, value 쌍을 배열로 반환([0] : 키, [1] : value)
 - delete(key) : 맵의 요소를 지움
 - clear() : 맵의 모든 요소를 지움
 
 ## 10.2 위크맵
 - Weak맵의 키는 반드시 객체여야 함
 - WeakMap의 키는 가비지 콜렉션에 포함 될수 있음.
 - WeakMap은 이터러블이 아니며 clear 메서드가 없음 
 ~~~javascript
 	const SecretHolder = (function(){
 		const secrets = new WeakMap();
 		return class{
 			setSecret(secret){
 				secrets.set(this, secret);
 			}
 			getSecret(){
 				return secrets.get(this);
 			}
 		}
 	})();
 	
 	const a = new SecretHolder();
 	const b = new SecretHolder();
 	
 	a.setSecret('secret A');
 	b.setSecret('secret B');
 	
 	console.log(`a secret ${a.getSecret()}`);
 	console.log(`b secret ${b.getSecret()}`);
 ~~~

## 10.3 셋
- 셋은 중복을 허용하지 않는 데이터 집합
- add(value) : 셋에 값을 추가, 이미 있는 값인 경우 아무일도 일어나지 않음.
- size 프로퍼티 : 셋의 사이즈
- delete(value) : 셋의 value 제거
 	
## 10.4 위크셋
- WeakSet의 키는 반드시 객체여야 함
- WeakSet의 키는 가비지 콜렉션에 포함 될수 있음.
- WeakSet은 이터러블이 아니며 clear 메서드가 없음 
 
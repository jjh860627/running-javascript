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
 		constructor(make, model){
 			this.make = make;
 			this.model = model;
 			this.userGears = ['P', 'N', 'R', 'D'];
 			this.userGear = this.userGears[0];
 		}
 		
 		shift(gear){
 			if(this.userGears.indexOf(gear) < 0){
 				throw new Error(`Invalid gear: ${gear}`);
 			}
 			this.userGear = gear;
 		}
	}
	~~~
 	
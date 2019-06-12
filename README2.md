# Chapter12.이터레이터와 제너레이터
ES6에 처음 도입된 개념으로 이터레이터는 제너레이터에 의존하는 개념
## 12.1 이너레이션 프로토콜
- 이터레이터 프로토콜은 모든 객체를 이터러블 객체로 바꿀 수 있음
- 이터레이션 프로토콜 조건
   - 심볼메소드 Symbol.iterator가 존재
   - Symbol.iterator 메소드는 value와 done 프로퍼티가 있는 객체를 반환하는 next() 메서드가 존재
	~~~javascript
	class Log{
		constructor(){
			this.messages = [];
		}
		
		add(message){
			this.messages.push({message, timestamp: Date.now() });
		}
		
		[Symbol.iterator](){
			//return this.messages.values();
			let i = 0;
			const messages = this.messages;
			return { //next()메서드가 존재하는 객체 반환
				next(){  //value와 done 프로퍼티가 있는 객체를 반환하는 next 메서드
					if(i >= messages.length){
						return {value: undefined, done: true};
					}else{
						return {value : messages[i++], done: false};
					}
				}
			}
		}
	}
	~~~
## 12.2 제너레이터
- 이터레이터를 사용해 자신의 실행을 제어하는 함수
- 함수의 실행을 개별적 단계로 나누어 함수의 실행을 제어
- 실행 중인 함수와 통신 가능
- 제너레이터는 언제든 제너레이터 함수의 호출자에게 제어권을 yield 키워드로 넘길 수 있음
- 제너레이터는 호출한 즉시 실행 되는 것이 아니라 이터레이터를 반환하고 이터레이터의 next()메서드를 호출함에 따라 실행 됨.
- 제너레이터를 만들때는 function 키워드 뒤에 '*'을 붙이며 화살표 표기법 사용불가
	~~~javascript
	function* rainbow(){
		yield '빨';
		yield '주';
		yield '노';
		yield '초';
		yield '파';
		yield '남';
		yield '보';
	}
	
	const it = rainbow(); //iterator 를 반환
	for(let color of it){
		console.log(color);
	}
	~~~
	
	~~~javascript
		function* interrogate(){
			const name = yield "What is your name?";
			const color = yield "What is your favorite color?";
			return `${name}'s favorite color is ${color}.`;
		}
		
		const it = interrogate();
		console.log(it.next());
		console.log(it.next('Hong'));
		console.log(it.next('Orange'));
		
	~~~
	
	- yield 문은, 
	
	~~~javascript
	function* abc(){
		yield 'a';
		yield 'b';
		return 'c';
	}
	
	const it = abc();
	console.log(it.next().value);
	console.log(it.next().value);
	console.log(it.next().value);
	
	for(let l of abc()){
		console.log(l); //c는 출력 되지 않음.
	}
	
	
# Chapter13.함수와 추상적 사고


## 13.1 서브루틴으로서의 함수
- 서브루틴 : 반복되는 작업의 일부를 떼어내서 이름을 붙이고, 언제든 그 이름을 부르기만 하면 실행
- 함수의 이름은 다른사람이 함수 이름만 봐도 함수에 대해 이해할 수 있도록 주의깊게 만들어야 함

## 13.2 값을 반환하는 서브루틴으로서의 함수
~~~javascript
 	function isCurrentYearLeapYear(){ //Boolean을 반환하는 함수는 isXXX와 같이 명명하는것이 좋음
 		const year = new Date().getFullYear();
 		if(year % 4 !== 0) return false;
 		else if(year % 100 !== 0) return true;
 		else if(year % 400 !== 0) return false;
 		else return true;
 	}
~~~

## 13.3 함수로서의 함수
함수는 항상 순수한 함수를 사용하는 습관을 들이는 것이 좋음
- 순수한함수 : 함수에 입력이 들어가면 결과가 나오며, 같은 입력에는 같은 결과를 반환하는 함수
~~~ javascript
	function isLeapYear(year){
		if(year % 4 !== 0) return false;
 		else if(year % 100 !== 0) return true;
 		else if(year % 400 !== 0) return false;
 		else return true;
	}
~~~
- 순수하지 않은 함수
~~~ javascript
	const colors = ['빨', '주', '노', '초', '파', '남', '보'];
	let colorIndex = -1;
	function getNextRainbowColor(){ 
		if(++colorIndex >= colors.length) colorIndex = 0; //++colorIndex로 전역변수 colorIndex의 값을 바꾸는 부수효과가 있음
		return colors[colorIndex]; //매개변수가 없어 입력이 같아도 결과가 매번 다름
	}
~~~
~~~javascript
	//Iterator로 바꾸는 것이 효과적임
	const getRainbowColorIter = (function(){
		const colors = ['빨', '주', '노', '초', '파', '남', '보'];
		let colorIndex = -1;
		return {
			next(){
				if(++colorIndex >= colors.length){
					colorIndex = 0;
				}
				return {value : colors[colorIndex], done : false};
			}
		}
	})();
~~~
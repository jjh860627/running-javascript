# Chapter12.���ͷ����Ϳ� ���ʷ�����
ES6�� ó�� ���Ե� �������� ���ͷ����ʹ� ���ʷ����Ϳ� �����ϴ� ����
## 12.1 �̳ʷ��̼� ��������
- ���ͷ����� ���������� ��� ��ü�� ���ͷ��� ��ü�� �ٲ� �� ����
- ���ͷ��̼� �������� ����
   - �ɺ��޼ҵ� Symbol.iterator�� ����
   - Symbol.iterator �޼ҵ�� value�� done ������Ƽ�� �ִ� ��ü�� ��ȯ�ϴ� next() �޼��尡 ����
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
			return { //next()�޼��尡 �����ϴ� ��ü ��ȯ
				next(){  //value�� done ������Ƽ�� �ִ� ��ü�� ��ȯ�ϴ� next �޼���
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
## 12.2 ���ʷ�����
- ���ͷ����͸� ����� �ڽ��� ������ �����ϴ� �Լ�
- �Լ��� ������ ������ �ܰ�� ������ �Լ��� ������ ����
- ���� ���� �Լ��� ��� ����
- ���ʷ����ʹ� ������ ���ʷ����� �Լ��� ȣ���ڿ��� ������� yield Ű����� �ѱ� �� ����
- ���ʷ����ʹ� ȣ���� ��� ���� �Ǵ� ���� �ƴ϶� ���ͷ����͸� ��ȯ�ϰ� ���ͷ������� next()�޼��带 ȣ���Կ� ���� ���� ��.
- ���ʷ����͸� ���鶧�� function Ű���� �ڿ� '*'�� ���̸� ȭ��ǥ ǥ��� ���Ұ�
	~~~javascript
	function* rainbow(){
		yield '��';
		yield '��';
		yield '��';
		yield '��';
		yield '��';
		yield '��';
		yield '��';
	}
	
	const it = rainbow(); //iterator �� ��ȯ
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
	
	- yield ���� ���ʷ������� ������ ���̴��� ���ʷ����͸� ������ ����.
	- �ݴ�� return ���� done�� true�� �ǰ� value�� return �� ��ȯ�ϴ� ���� ��.
	- ����, ���ʷ����Ϳ����� �߿��� ���� return �ؼ��� �ȵ�.(yield�� �����)
	
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
		console.log(l); //c�� ��� ���� ����.
	}
	
	
# Chapter13.�Լ��� �߻��� ���


## 13.1 �����ƾ���μ��� �Լ�
- �����ƾ : �ݺ��Ǵ� �۾��� �Ϻθ� ����� �̸��� ���̰�, ������ �� �̸��� �θ��⸸ �ϸ� ����
- �Լ��� �̸��� �ٸ������ �Լ� �̸��� ���� �Լ��� ���� ������ �� �ֵ��� ���Ǳ�� ������ ��

## 13.2 ���� ��ȯ�ϴ� �����ƾ���μ��� �Լ�
~~~javascript
 	function isCurrentYearLeapYear(){ //Boolean�� ��ȯ�ϴ� �Լ��� isXXX�� ���� ����ϴ°��� ����
 		const year = new Date().getFullYear();
 		if(year % 4 !== 0) return false;
 		else if(year % 100 !== 0) return true;
 		else if(year % 400 !== 0) return false;
 		else return true;
 	}
~~~

## 13.3 �Լ��μ��� �Լ�
�Լ��� �׻� ������ �Լ��� ����ϴ� ������ ���̴� ���� ����
- �������Լ��� �Լ��� �Է��� ���� ����� ������, ���� �Է¿��� ���� ����� ��ȯ�ϴ� �Լ�
- �������Լ��� ���� �ڵ带 �׽�Ʈ�ϱ� ����, �����ϱ� ����, �����ϱ⵵ �� ����.
~~~ javascript
	function isLeapYear(year){
		if(year % 4 !== 0) return false;
 		else if(year % 100 !== 0) return true;
 		else if(year % 400 !== 0) return false;
 		else return true;
	}
~~~
- �������� ���� �Լ�
~~~ javascript
	const colors = ['��', '��', '��', '��', '��', '��', '��'];
	let colorIndex = -1;
	function getNextRainbowColor(){ 
		if(++colorIndex >= colors.length) colorIndex = 0; //++colorIndex�� �������� colorIndex�� ���� �ٲٴ� �μ�ȿ���� ����
		return colors[colorIndex]; //�Ű������� ���� �Է��� ���Ƶ� ����� �Ź� �ٸ�
	}
~~~
~~~javascript
	//Iterator�� �ٲٴ� ���� ȿ������
	const getRainbowColorIter = (function(){
		const colors = ['��', '��', '��', '��', '��', '��', '��'];
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

## 13.4 �Լ��� ��ü��
- �ڹٽ�ũ��Ʈ�� �Լ��� Function ��ü�� �ν��Ͻ�
- typeof �Լ� = "function"

## 13.5 IIFE�� �񵿱��� �ڵ�
- IIFE�� ����ϴ� ����� �ϳ��� �񵿱��� �ڵ尡 ��Ȯ�� ������ �� �ֵ��� �� ������ �� �������� ����� ����.
~~~javascript
	var i;
	for(i=5; i>=0; i--){
		setTimeout(function(){
			console.log(i===0 ? "go!" : i); //���� �������� ���� ���� for ������ ��� �� ���� ��(-1) 
		}, (5-i) * 1000);
	}
~~~
~~~javascript
	var i;
	for(i=5; i>=0; i--){
		(function(i){
			setTimeout(function(){
				console.log(i===0 ? "go!" : i); //���� �������� ���� ���� for ������ ��� �� ���� ��(-1) 
			}, (5-i) * 1000);
		})(i);
	}
	
	for(let i=5; i>=0; i--){
		setTimeout(function(){
			console.log(i===0 ? "go!" : i); //���� �������� ���� ���� for ������ ��� �� ���� ��(-1) 
		}, (5-i) * 1000);
	}
~~~

## 13.6 �����μ��� �Լ�
- �Լ��� �ٸ� ������ ���������� �̸����� ���� �Ҽ� ����
- �Լ��� ����Ű�� ������ ����� ������ ���� �� ����
- �迭�� �Լ��� ���� �� ����.
- �Լ��� ��ü�� ������Ƽ�� ��밡��
- �Լ��� �Լ��� ���� ����
- �Լ��� �Լ��� ��ȯ ����
- �Լ��� �Ű������� �޴� �Լ��� ��ȯ�ϴ°͵� ����
~~~javascript
	function addThreeSquareAddFiveTaskSquareRoot(x){
		return Math.sqrt(Math.pow(x+3, 2)+5);
	}
	
	const f = addThreeSquareAddFiveTaskSquareRoot;
	const answer = (f(5) + f(2) + f(7));
~~~
## 13.6.1 �迭 ���� �Լ�
## 13.6.2 �Լ��� �Լ� ����
- �񵿱��� ���α׷����� ���� �Լ��� �Լ��� ����(�ݹ�)
~~~javascript
	function sum(arr, f){
		if(typeof f != 'function') f = x => x; //function�� ���� ��� �Ű����� �״�� ��ȯ�ϴ� �Լ� ���
		return arr.reduce((a,x) => a += f(x), 0);
	}
	console.log(sum([1,2,3]));
	console.log(sum([1,2,3], x => x*x));	
	console.log(sum([1,2,3], x => Math.pow(x, 3)));
~~~
## 13.6.3 �Լ��� ��ȯ�ϴ� �Լ�
~~~javascript
	function sum(arr, f){
		if(typeof f != 'function') f = x => x; //function�� ���� ��� �Ű����� �״�� ��ȯ�ϴ� �Լ� ���
		return arr.reduce((a,x) => a += f(x), 0);
	}
	function newSummer(f){
		return arr => sum(arr, f);
	}
	
	const sumOfSquares = newSummer(x => x*x);
	const sumOfCubes = newSummer(x => Math.pow(x,3));
	console.log(sumOfSquares([1,2,3]));
	console.log(sumOfCubes([1,2,3]));
~~~
##13.7 ���
�ڱ� �ڽ��� ȣ�� �ϴ� �Լ��� ���� ������ �־�� ��.
~~~javascript
	function fact(n){
		if(n === 1) return 1;
		return n * fact(n-1);
	}
~~~

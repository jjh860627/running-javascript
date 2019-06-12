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
	
	- yield ����, 
	
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
- �������Լ� : �Լ��� �Է��� ���� ����� ������, ���� �Է¿��� ���� ����� ��ȯ�ϴ� �Լ�
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
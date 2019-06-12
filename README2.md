# Chapter12.���ͷ����Ϳ� ���ʷ�����
ES6�� ó�� ���Ե� �������� ���ͷ����ʹ� ���ʷ����Ϳ� �����ϴ� ����
## 12.1 �̳ʷ��̼� ��������
- ���ͷ����� ���������� ��� ��ü�� ���ͷ��� ��ü�� �ٲ� �� ����
- ���ͷ��̼� �������� ����
   - �ɺ��޼ҵ� Symbol.iterator�� ����
   - Symbol.iterator �޼ҵ�� value�� done ������Ƽ�� �ִ� ��ü�� ��ȯ�ϴ� next() �޼��尡 ����


-  ��ü�� ������Ƽ ������ �迭�� �޸� ������ ���� ���� ����
	### 9.1.1 for...in
	- ��ü ������Ƽ ������ for ... in�� ��� (Iterable ��ü�� for ... of ���)
	- for ... in �������� Ű�� �ɺ��� ������Ƽ�� ���Ե��� ����
	- ���ܻ�Ȳ�� ���ϱ� ���� hasOwnProperty�� üũ�ϴ°� ����
 	### 9.1.2 Object.keys()
	- ��ü���� ���� ������ ���ڿ� ������Ƽ�� �迭�� ��ȯ
	- hasOwnProperty�� üũ�� �ʿ�� ����.
 ## 9.2 ��ü���� ���α׷���
- �����Ϳ� ����� �������� ���� ���� �з�����
 	### 9.2.1 Ŭ������ �ν��Ͻ� ����
 	- Ŭ������ �����ڴ� constructor(){} �� ���� ����
 	- �ش� Ŭ������ �ν��Ͻ��� ���� ���� "new" Ű���� ���
 	- "instanceof" �����ڸ� �̿��� ��ü�� Ŭ������ �ν��Ͻ����� Ȯ�� ����(java�� ����)

 	~~~javascript
 	class Car{
 		//������
 		constructor(make, model){
 			this.make = make;
 			this.model = model;
 			this.userGears = ['P', 'N', 'R', 'D'];
 			this.userGear = this.userGears[0];
 		}
 		//�޼ҵ忡�� function Ű���带 ���� ����.
 		shift(gear){
 			if(this.userGears.indexOf(gear) < 0){
 				throw new Error(`Invalid gear: ${gear}`);
 			}
 			this.userGear = gear;
 		}
	}
	~~~
	- javascript���� ���������ڰ� ����
	- ������Ƽ�� ��ȣ�ؾ� �Ѵٸ� �������� �̿��ϰ� WeakMap �ν��Ͻ��� ���
	
	~~~javascript
	const Car = (function(){
		const carProps = new WeakMap(); //WeakMap ���
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
	### 9.2.2 Ŭ������ �Լ���
	- ES5 ������ Car Ŭ������ �Ʒ��� ���� ����
	~~~javascript
	function Car(make, model){
		this.make = make;
		this.model = model;
		this.userGears = ['P', 'N', 'R', 'D'];
		this.userGear = this.userGears[0];
	}
	- ES6�� Ŭ������ ��쵵 typeof �������� ����� function��(������ class�� ���� new Ű���带 �ݵ�� ����ؾ���)
	~~~
	### 9.2.3 ������Ÿ��
	- Ŭ������ �ν��Ͻ����� ����� �� �ִ� �޼��� = ������Ÿ�� �޼���
	- ������Ÿ�� �޼���� Car.prototype.shift ó�� ǥ��
	- Ŭ������ Car ó�� �׻� ù���ڸ� �빮�ڷ� ǥ���ϴ°� �Ϲ���(�Ľ�Į ǥ���)
	- ���� ����ġ : ��ü�� ������Ƽ�� �޼��忡 �����Ϸ���  �� �� �ش� ��ü�� ���� ���� ������ ��ü�� ������Ÿ�Կ��� �ش� ������Ƽ�� �޼��带 ã�� ������Ÿ�Կ��� ã�� ���ϸ� ������Ÿ���� ������Ÿ�Ա��� ã��(�θ�)
	- �ν��Ͻ����� ������Ÿ�Կ� �����ϴ� �޼��峪 ������Ƽ�� �����ϸ� ������Ÿ�Կ� �ִ� ���� ������ ȿ���� ��Ÿ��
	### 9.2.4 ���� �޼���
	- static Ű���带 ����� �޼���
	- Ư�� �ν��Ͻ��� ������� �ʰ� Ŭ������.xxx() �� ���� ȣ���ϴ°� ����.
	
	~~~javascript
	class Car{
		static getNextVin(){
			return Car.nextVin++; //this.nextVin�̶�� �ᵵ ������ ���� �޼������� ����ϱ� ���� Car�� ��
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
 
 	### 9.2.5 ���
 	- Ŭ������ �ν��Ͻ��� Ŭ������ ����� ��� ���
 	- �ڹٽ�ũ��Ʈ�� ������Ÿ�� ü��(��ü => ��ü�� ������Ÿ�� => ������Ÿ���� ������Ÿ��(��ӵ� �θ�)���� �޼��峪 ������Ƽ�� ã��
 	- extends Ű���带 ����� Ŭ������ ��ӹ���.
 	- �ݵ�� �����ڿ��� ��ӹ��� �θ�Ŭ������ ������(super())�� ȣ���� �����.(�ڹ��� ���� default�������� ���� ��������)
 	
 	### 9.2.6 ������
 	- ��ü���� ���� ���� ����Ŭ������ ����� �ν��Ͻ��� ������ ����
 	- instanceof �����ڷ� ��ü�� Ŭ���� Ȯ��
 	- �ڹٽ�ũ��Ʈ�� ��� ��ü�� Object Ŭ������ ����Ŭ������.

 	### 9.2.7 ��ü ������Ƽ ���� �ٽú���
 	- obj.hasOwnProperty(x)�� obj�� ������Ƽ x�� �ִٸ� true�� ��ȯ, ���ǵ��� �ʾҰų� ������Ÿ�� ü�ο��� ���ǵ� ���� false ��ȯ
 	- ������Ƽ�� ������Ÿ�Կ� �������� ���ϵ��� �����ϴ� ��ġ�� �����Ƿ� �׻� "hasOwnProperty"�� ����ϴ� ���� ����.
 	- ����Ŭ���� �����忡�� ������ ������Ƽ�� ����Ŭ���� �ν��Ͻ����� ���ǵ�(������Ÿ�Կ� ���ǵǴ°��� �ƴ�)
 	
 	~~~javascript
 		class Super{
 			constructor(){
 				this.name = 'Super';
 				this.isSuper = true;
 			}
		}
		//Ŭ������ ������Ÿ�Կ� sneaky ������Ƽ �Ҵ�
		Super.prototype.sneaky = 'not recommended'; //�������� ����
		
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
	
	### 9.2.8 ���ڿ� ǥ��
	- Object Ŭ������ toString() �����Ͽ� ���
	
	### 9.3 ���߻��, �ͽ���, �������̽�
	- �ڹٽ�ũ��Ʈ���� ���߻���� �������� ������ �������̽� ���� ����.
	- �ڹٽ�ũ��Ʈ�� ���� ����� �ʿ��� ������ ���� �ش����� ������ ������ �ͽ���
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
		
		makeInsurable(Car.prototype); //Car�� �ν��Ͻ��� �ƴ� prototype ��ü�� �ѱ����ν� ��� Car Instance�� �ϰ�����
	~~~
 	
# Chapter10. �ʰ� ��

## 10.1 ��
 - set(key, value) : �ʿ� �� �߰�, ���� Ű�� �ٽ� set�ϴ� ��� value�� ��ü��
 - get(key) : �ʿ� ����� �ش� Ű���� �ش��ϴ� value�� ������
 - has(key) : �ʿ� Ű�� �����ϴ��� Ȯ��
 - size ������Ƽ : ���� ������ 
 - keys() : ���� Ű�� Iterable�� ��ȯ
 - values() : ���� value�� Iterable�� ��ȯ
 - entirs() : ���� key, value ���� �迭�� ��ȯ([0] : Ű, [1] : value)
 - delete(key) : ���� ��Ҹ� ����
 - clear() : ���� ��� ��Ҹ� ����
 
 ## 10.2 ��ũ��
 - Weak���� Ű�� �ݵ�� ��ü���� ��
 - WeakMap�� Ű�� ������ �ݷ��ǿ� ���� �ɼ� ����.
 - WeakMap�� ���ͷ����� �ƴϸ� clear �޼��尡 ���� 
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

## 10.3 ��
- ���� �ߺ��� ������� �ʴ� ������ ����
- add(value) : �¿� ���� �߰�, �̹� �ִ� ���� ��� �ƹ��ϵ� �Ͼ�� ����.
- size ������Ƽ : ���� ������
- delete(value) : ���� value ����
 	
## 10.4 ��ũ��
- WeakSet�� Ű�� �ݵ�� ��ü���� ��
- WeakSet�� Ű�� ������ �ݷ��ǿ� ���� �ɼ� ����.
- WeakSet�� ���ͷ����� �ƴϸ� clear �޼��尡 ���� 
 
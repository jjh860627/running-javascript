# Chapter9. ��ü�� ��ü���� ���α׷���

## 9.1 ������Ƽ ����
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
 
 	### 9.2.4 ���
 	- Ŭ������ �ν��Ͻ��� Ŭ������ ����� ��� ���
 	- �ڹٽ�ũ��Ʈ�� ������Ÿ�� ü��(��ü => ��ü�� ������Ÿ�� => ������Ÿ���� ������Ÿ��(��ӵ� �θ�)���� �޼��峪 ������Ƽ�� ã��
 	
 	
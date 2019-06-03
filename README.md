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
	
	
 
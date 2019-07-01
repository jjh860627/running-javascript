# Chapter15.날짜와 시간
## 15.1 날짜, 타임존, 타임스탬프, 유닉스 시간
- 자바스크립트에서 Date 인스턴스는 모두 유닉스 시간 원점(1970년 1월 1일 0시 0분 0초) 으로부터 몇 밀리초가 지났는지 나타내는 숫자
- 숫자형 표현(유닉스 타임스탬프)이 필요하면 valudOf() 메서드를 쓰면됨.
	    
## 15.2 Date 객체 만들기 
~~~ javascript
	new Date(); //현재 날짜
	
	//년월일 시분초를 입력하여 생성
	new Date(2015,0); //월은 0부터 시작
	new Date(2015, 1, 14);
	new Date(2015, 1, 14, 13);
	new Date(2015, 1, 14, 13, 30);
	new Date(2015, 1, 14, 13, 30, 5);
	new Date(2015, 1, 14, 13, 30, 5, 500);
	
	//유닉스 타임스탬프로 날짜 생성
	new Date(0);	
	new Date(1463443200000);
	
	//유닉스 시간 원점 이전의 날짜를 구할 때 
	new Date(-365 * 24 * 60 * 60 * 1000)
	
	//날짜 문자열 해석(표준시를 기준으로 함)
	new Date('June 14, 1903'); //지역 표준시로 생성됨
	new Date('June 14, 1903 GMT-0000'); //GMT0000 = UTC
~~~	
	
## 15.3 Moment.js
- Javascript의 Date 객체의 기능이 불충분해 Moment.js 라이브러리를 사용하면 좋음
- <script src="//cdnjs.cloudflare.com/ajax/libs/moment-timezone/0.4.0/moment-timezone.min.js"></script>
- npm install --save moment-timezone

## 15.5 날짜 데이터 만들기
- 타임존을 명시하지 않고 날짜를 생성할 때는 어느 타임존이 사용되는지 생각해야함
- 타임존이 명시되지 않는 경우는 시스템(브라우저 또는 서버)의 타임존을 기본으로 생성됨

### 15.5.1 서버에서 날짜 생성하기
- 서버에서 날짜를 생성할 때는 항상 UTC를 사용하거나 타임존을 명시하는 것이 좋음.
~~~ javascript
	const d = new Date(Date.UTC(2016, 4, 27)); //UTC 날짜를 사용, Date.UTC 메서드는 Timestamp를 반환
~~~

### 15.5.2 브라우저에서 날짜 생성하기
- 다른 타임존의 날짜를 처리해야 하는 경우는 Moment.js 사용하하여 타임존을 변경

## 15.6 날짜 데이터 전송하기
- 자바스크립트의 Date 인스턴스는 날짜를 저장할떄 UTC를 기준으로 유닉스 타임스탬프를 저장하므로 Date 객체를 그냥 전송해도 안전함
- JSON을 사용
~~~ javascript
	const before = {d : new Date()};
	before.d instanceof Date;
	const json = JSON.stringify(before);
	const after = JSON.parse(json);
	after.d instanceof Date;
	typeof after.d;
	
	//문자열 => Date 객체로 복구
	after.d = new Date(after.d);
	after.d instanceof Date;
	
	//Timestamp(숫자형) 으로 전송해도 됨.
	const before = {d: new Date().valueOf() };
	typeof before.d;
	const json = JSON.stringify(before);
	const after = JSON.parse(json);
	typeof after.d;
	const d = new Date(after.d);
~~~

## 15.7 날짜 형식
- 자바스크립트 Date 객체에서 제공하는 날짜 형식은 다양하지 않아 Moment.js 를 사용하는 것이 좋음
- Moment.js의 format 메서드를 사용

~~~javascript
	const d = new Date(Date.UTC(1930, 4, 10));
	
	d.toLocaleDateString();
	d.toLocaleFormat();   //없음...
	d.toLocaleTimeString();
	d.toTimeString();
	d.toUTCString();
	
	moment(d).format("YYYY-MM-DD");
	moment(d).format("YYYY-MM-DD HH:mm");
	moment(d).format("YYYY-MM-DD HH:mm Z");
	moment(d).format("YYYY-MM-DD HH:mm [UTC]Z");
	moment(d).format("YYYY년 M월 D일 HH:mm");
	
	moment(d).format("dddd, MMMM [the] Do, YYYY");
	moment(d).format("h:mm a");
	
~~~
	
## 15.8 날짜 구성요소
~~~javascript
	const d = new Date(Date.UTC(1815, 9, 10));
	
	//지역 표준시 기준 메서드
	d.getFullYear();
	d.getMonth();
	d.getDate();
	d.getDay();
	d.getHours();
	d.getMinutes();
	d.getSeconds();
	d.getMilliseconds();
	
	//UTC 기준 메서드
	
	d.getUTCFullYear();
	d.getUTCMonth();
~~~

## 15.9 날짜 비교
- Date 객체는 날짜를 숫자(유닉스 타임스탬프)로 저장하므로 숫자에 쓰는 비교 연산자를 이용하여 비교
~~~javascript
	const d1 = new Date(1996, 2, 1);
	const d2 = new Date(2009, 4, 27);
	
	d1 > d2
	d1 < d2
~~~

## 15.10 날짜 연산
- 날짜는 숫자이므로 날짜에서 날짜를 빼면 몇미리초가 지나갔는지 알수 있음
~~~javascript
	const d1 = new Date(1996, 2, 1);
	const d2 = new Date(2009, 4, 27);
	const msDiff = d2 - d1;
	const daysDiff = msDiff/1000/60/60/24;
~~~
- Moment.js 에는 날짜를 빼거나 더하는 데 유용한 메서드들이 있음.
~~~javascript
	let m = moment();
	m.add(3, 'days'); //3일을 더함
	m.subtract(2, 'years'); //2년을 뺌
	
	m = moment();
	m.startOf('year'); //올해의 1월 1일로 셋팅
	m.endOf('month'); //이달의 마지막 날로 셋팅
~~~
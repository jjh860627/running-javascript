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

## 15.11 사용자가 알기 쉬운 상대적 날짜
- Moment.js를 이용하여 상대적 날짜 표시
~~~ javascript
	moment().subtract(10, 'seconds').fromNow();
	moment().subtract(44, 'seconds').fromNow();
	moment().subtract(45, 'minutes').fromNow();
	moment().subtract(5, 'hours').fromNow();
	moment().subtract(15, 'days').fromNow();
~~~

# Chapter17. 정규표현식
- 정교한 문자열 매칭 기능 제공
- 문자열 교체도 가능

## 17.1 부분 문자열 검색과 대체
- String.prototype 메서드에도 검색과 교체 기능이 존재
~~~ javascript
	const input = "As I was going to Saint Ives";
	input.startsWith("As");
	input.endsWith("Ives");
	input.startsWith("going", 9); //인덱스 9에서 시작하는 것으로 간주
	input.endsWith("going", 14); //인덱스 14를 문자열의 끝으로 간주
	input.includes("going");
	input.includes("going", 10); //인덱스 10에서부터 찾음
	input.indexOf("going");
	input.indexOf("going", 10); //인덱스 10에서 부터 찾으나 결과가 10부터 계산된 인덱스는 아님
	input.indexOf("nope");
	
	//대소문자를 구분하지 않으므로 소문자로 변경하여 비교
	input.toLowerCase().startsWith("as");
	
	//문자열 교체
	const output = input.replace("going", "walking");
~~~

## 17.2 정규식 만들기
- 정규식은 RegExp 클래스를 이용해서 생성
- 슬래시로 감싼 형태인 리터럴 문법도 가능
~~~ javascript
	const re1 = /going/;
	const re2 = new RegExp("going");
	
## 17.3 정규식 검색
~~~ javascript
	const input = "As I was going to Saint Ives";
	//세글자 이상인 단어를 찾는 정규식
	//i  : insensitive, g: global(전역으로 검색, 이 키워드가 없는 경우 일치하는 문자열을 찾는 동시에 검색을 중지함
	const re = /\w{3,}/ig; 
	
	//String.prototype 의 메서드를 이용해서 찾는 방법
	input.match(re);	//매칭하는 단어를 배열로 반환
	input.search(re); //매칭하는 첫번째 단어의 인덱스를 반환
	
	//정규식(RegExp).prototype 의 메서드를 사용
	re.exec(input); //exec 메소드는 마지막 찾은 위치를 기억해 두었다가 다시 호출하면 그 위치부터 찾음.
	re.exec(input);
	re.exec(input);
	re.exec(input);
	re.exec(input);
	
	re.test(input); //매칭하는 단어가 하나라도 존재하는지 여부

	//정규식 리터럴을 그대로 사용해도 가능	
	input.match(/\w{3,}/ig);
	input.search(/\w{3,}/ig); 
	/\w{3,}/ig.test(input);
	/\w{3,}/ig.exec(input);
~~~

## 17.4 정규식을 사용한 문자열 교체
- String.prototype.replace 메서드에도 정규식을 사용하여 문자열 교체가 가능

~~~ javascript
	const input = "As I was going to Saint Ives";
	const output = input.replace(/\w{4,}/ig,'****');
~~~

## 17.5 입력 소비
- 정규식은 입력 문자열을 소비하는 패턴
- 정규식은 왼쪽에서 오른쪽으로 한글자씩 입력 문자열을 검증하는데 일치하지 않는 문자의 경우는 해당 문자를 소비하고 다음문자를 검증
- 일치할 가능성이 있는 문자가 연속되는 경우 문자열을 소비하지 않다가 일치하는 문자를 만나는 경우 해당 문자열을 모두 소비
- 정규식에 일치하지 않는 문자를 만나는 경우 맨 처음 한 문자만 소비하고 그 다음문자부터 다시 검증
- 일단 소비한 글자에 다시 돌아오는 일은 없음
~~~ javascript
	const input = 'XJANLIONATUREJXEELNP';
	input.match(/LION|ION|NATURE|EEL/g); 
	// 위의 경과는 ["LION", "EEL"] 만 검색 됨.
	// NATURE의 경우도 정규식에 일치하는 문자열이지만 LION을 찾으면서 LION문자들을 
	// 소비하게 되기 때문에 NATURE의 시작문자인 N은 소비대상에서 제외되기 때문
~~~

## 17.6 대체
- '|' 를 사용하여 여러 문자열을 대체하여 찾을 수 있음
~~~ javascript
	const html = 'HTML with <a href="\one">one line</a>, and some<area>test</area> Javascript.<script src="stuff.js">';
	const matches = html.match(/area|a|link|script|source/ig); //대체 문자를 쓰는 경우 앞에서 부터 순서대로 찾기 때문에 a보다 area가 앞에 있어야 area를 찾을 수 있음
~~~

## 17.7 HTML 찾기
- HTML을 정규식으로 완벽히 분석할 수 없음
- HTML 분석 시에는 전용 파서를 사용해야 함.

## 17.8 문자셋
~~~ javascript
	const beer99 = "99 bottles of beer on the wall take 1 down and pass it around -- 98 bottols of beer on the wall.";
	let matches = beer99.match(/0|1|2|3|4|5|6|7|8|9/g);
	
	matches = beer99.match(/[0123456789]/g); //대체문자를 쓰지 않아도 됨
	matches = beer99.match(/[0-9]/g); //범위를 사용
	
	matches = beer99.match(/\-[0-9a-z]/ig); //하이픈은 메타 문자에 포함되기 때문에 \를 붙여 줘야함
	
	matches = beer99.match(/^\-[0-9a-z]/ig); //^를 쓰는 경우 해당 문자셋을 제외하고 찾음
~~~

## 17.9 자주 쓰는 문자셋
- 자주 쓰이는 문자셋은 단축 표기가 있음.(클래스라고 부름)

| 값 | 의미 | 기본값 |
|---|:---:|---:|
| `static` | 유형(기준) 없음 / 배치 불가능 | `static` |
| `relative` | 요소 자신을 기준으로 배치 |  |
| `absolute` | 위치 상 부모(조상)요소를 기준으로 배치 |  |
| `fixed` | 브라우저 창을 기준으로 배치 |  |



~~~
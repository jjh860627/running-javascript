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

| 문자셋 | 동등한 표현 | 노트 |
|---|:---:|---:|
| `\d` | [0-9] |  |
| `\D` | [^0-9] |  |
| `\s` | [ \t\v\n\r] | 탭, 스페이스, 세로 탭, 줄바꿈 |
| `\S` | [^ \t\v\n\r] |  |
| `\w` | [a-zA-Z_] |  |
| `\W` | [^a-zA-Z_] |  |

~~~ javascript
	const messyPhone = '(505) 555-1515';
	const neatPhone = messyPhone.replace(/\D/g, ''); //숫자를 제외한 모든 문자 제거
	
	const field = '   something    ';
	const valid = /\S/.test(field); //공백이 아닌 문자가 하나라도 있는지 검증
~~~

## 17.10 반복
- 반복 메타문자(수량자)는 얼마나 많이 일치해야 하는지 지정할 때 사용

| 반복 메타 문자 | 설명 | 예제 |
|---|:---:|---|
| `{n}` | 정확히 n개  | `/d{5}/`는 우편번호처럼 정확히 다섯자리 숫자에만 일치 |
| `{n,}` | 최소한 n개 | `/d{5,}/`는 다섯자리 이상의 숫자에만 일치 |
| `{n, m}` | n개 이상, m개 이하 | `/\d{2,5}`/는 숫자 2,3,4,5개에 일치 |
| `?` | 0개 또는 1개. {0,1}과 동등 | `/[a-z]\d?/i` 는 글자가 있고 그다음에 숫자가 없거나 한개 있는 경우 일치 |
| `*` | 갯수 상관없음 | `/[a-z]\d*/i`는 글자가 있고 그다음에 숫자가 없거나 있는 경우 일치 | |
| `+` | 하나 이상 | `/[a-z]\d+/i` 는 글자가 있고 그다음에 숫자가 한개 이상 있는 경우 일치 |

## 17.11 마침표와 이스케이프
- 정규식에서 마침표(.) 는 줄바꿈 문자를 제외한 모든 문자에 일치하는 특수문자
- 정규식 메타문자를 정규식 구문에 넣는 경우는 \를 앞에 붙인다.
- 줄바꿈 문자도 포함하여 모든문자를 찾는 경우 "[\s\S]"를 쓰면 됨.
~~~ javascript
	const input = "Address: 333 Main St., Anywhere, NY, 55532. Phone: 555-555-2525.";
	const match = input.match(/\d{5}.*/);
~~~

## 17.12 그룹
- 그룹을 사용하면 하위 표현식을 만들고 단위하나로 취급 가능
- 캡쳐하지 않는 그룹은 (?:[subexpression]) 형태로 만듬

~~~ javascript
const html = '<link rel="stylesheet" href="http://insecure.com/stuff.css">\n' + 
             '<link rel="stylesheet" href="https://secure.com/stuff.css">\n' + 
			 '<link rel="stylesheet" href="//anything.com/stuff.css">\n';
		
const matches = html.match(/(?:https?:?)?\/\/[a-z][a-z0-9-]+[a-z0-9]+/ig); 
~~~

## 17.13 소극적 일치, 적극적 일치






# Chapter18. 브라우저의 자바스크립트

## 18.1 ES5와 ES6
- 브라우저의 자바스크립트는 서버와 달리 사용자가 어떤 브라우저를 사용할지 모르기 때문에 당분간은 ES5를 사용해야함
- ES6로 개발하고 트랜스 컴파일러를 사용하여 여러 브라우저를 지원

## 18.2 문서 객체 모델
- 문서객체모델(DOM)은 HTML 문서의 구조를 나타내는 표기법
- DOM은 트리구조로 표현
- DOM 트리는 노드로 구성
- DOM 트리의 모든 노드는 Node 클래스의 인스턴스
- 모든 노드에는 nodeType, nodeName 프로퍼티가 존재

~~~ javascript
	//모든 DOM 노드를 순회하면서 print 
	function printDOM(node, prefix){
		console.log(prefix + node.nodeName);
		for(let i=0; i<node.childNodes.length; i++){
			printDOM(node.childNodes[i], prefix + '\t');
		}
	}
	printDOM(document, '');
~~~

## 18.3 용어사용
- 부모노드 : 직접적인 부모
- 자식노드 : 직접적인 자식
- 조상노드 : 부모, 부모의 부모등
- 자손노드 : 자식, 자식의 자식등

## 18.4 get메서드
- document.getElementById(id) : id를 이용해 요소를 찾음
- document.getElementsByClassName(class) : class 이름에 해당하는 요소들을 찾음
- document.getElementsByTagName(tag) : tag 이름에 해당하는 요소들을 찾음
- 위의 메서드들이 반환하는 컬렉션은 자바스크립트 배열이 아니라 HTMLCollection의 인스턴스임.

## 18.5 DOM 요소 쿼리
- querySelector와 querySelectorAll 메서드로 css 선택자를 이용해 요소를 찾을 수 있음
- 요소 선택자 사이에 스페이스를 넣으면 특정 노드의 자손인 요소를 찾을 수 있음
- 요소 선택자 사이에 '>'를 넣으면 특정 노드의 자식 노드를 찾을 수 있음.

## 18.6 DOM 요소 조작
- textContent 프로퍼티 : HTML 태그를 모두 제거하고 순수한 텍스트 데이터만 제공
- innerHTML : HTML 태그를 그대로 제공

~~~ javascript
	const para = document.getElementsByTagName('p')[0];
	para.textContent;
	para.innerHTML;
	para.textContent = "Modified HTML file"; 
	para.innerHTML = "Modified HTML file"; 
~~~

## 18.7 새 DOM 요소 만들기
- document.createElement(태그명) 을 이용하여 Element 생성
- 생성된 Element는 DOM에 자동 추가 되지 않음.
- 생성된 Element를 추가 하기 위해서는 아래 두 메소드를 이용
	- insertBefore(삽입할 요소, 삽입할 위치를 지정하는 요소)
	- appendChild(삽입할 요소) : 마지막 자식요소로 추가 됨
    
## 18.8 요소 스타일링
- 요소프러퍼티를 직접 수정하는 것보다는 CSS 클래스를 이용하는 편이 더 좋음.
- 모든 요소에는 classList 프로퍼티가 존재
- classList 프로퍼티의 add/remove 메서드로 클래스를 추가/삭제 가능

~~~ javascript
    .highlight {
        background : #ff0;
        font-style : italic;
    }
    
    function highlightParas(containing){
        if(typeof containing === 'string'){
            containing = new RegExp(`\\b${containing}\\b`, 'i');
        }
        const paras = document.getElementsByTagName('p');
        console.log(paras);
        for(let p of paras){
            if(!containing.test(p.textContent)) continue;
            p.classList.add('highlight');
                
        }
    }
    highlightParas('unique');
    
     function removeParaHighlights(){
    		const paras = document.querySelectorAll('p.highlight');
    		for(let p of paras){
    			p.classList.remove('highlight');
    		}
     }
~~~

## 18.9 데이터 속성
- 데이터 속성을 이용하여 요소에 임의의 데이터를 추가가능
- HTML에서 데이터 속성(data-) 추가됨
- document.querySelectorAll 을 이용해 [data-xxx="XXX"] 형태로 요소 검색 가능
- 요소의 dataset 프로퍼티로 데이터 속성에 접근 가능
- dataset 프로퍼티를 이용하여 데이터를 추가/삭제가 가능

~~~ javascript
	const highlightActions = document.querySelectorAll('[data-action="highlight"]');
	console.log(highlightActions[0].dataset);
	highlightActions[0].dataset.cool = "cool!!";
~~~

## 18.10 이벤트
- 모든 요소에는 addEventListener 라는 메서드가 존재하며 이 메서드를 통해 이벤트가 일어났을 때 호출할 함수를 지정가능
- 호출할 함수는 Event 타입의 객체 하나만 매개변수로 받으며 이벤트에 관한 정보가 모두 포함됨
- 이벤트 모델은 이벤트 하나에 여러가지 함를 연결할 수 있도록 설계
- 기본핸들러가 지정된 이벤트의 경우 event.preventDefault() 메서드를 호출하면 기본핸들러의 동작을 막을 수 있음
~~~ javascript
	const hightlightActions = document.querySelectorAll('[data-action="highlight"]');
	for(let a of hightlightActions){
		a.addEventListener('click', evt =>{
			evt.preventDefault();
			highlightParas(a.dataset.containing);
	    });
	}
	
	const removeHightlightActions = document.querySelectorAll('[data-action="removeHighlights"]');
	for(let a of removeHightlightActions){
		a.addEventListener('click', evt => {
			evt.preventDefault();
			removeParaHighlights();
	    });
	}
~~~

### 18.10.1 이벤트 버블링과 캡처링
- HTML은 계층적이므로 이벤트를 여러곳에서 처리할 수 있음
- 캡처링은 먼 조상부터 이벤트가 일어난 요소까지 내려오면서 이벤트 핸들 하는 방법
- 버블링은 이벤트가 일어난 요소에서 시작해서 거슬러 올라가면서 이벤트 핸들 하는 방법
- HTML 이벤트 모델에서는 캡처링 => 버블링 순서대로 이벤트를 핸들함
- 이벤트 핸들러에서 다른 핸들러가 어떻게 호출 될지 영향을 주는 방법
	- event.preventDefault() : 이벤트를 취소함. 취소된 이벤트는 계속 전달되기는 하지만 defaultPrevented 프로퍼티가 true 바뀐채 전달됨. 브라우저의 기본 이벤트 핸들러는 defaultPrevented 프로퍼티를 체크하고 이벤트를 실행하므로 실행하지만, 프로그래머가 추가한 이벤트 핸들러에서는 defaultPrevented를 체크하지 않았다면 계속 이벤트가 수행됨.
	- event.stopPropagation() : 이벤트를 현재 요소에서 끝내고 더는 전달되지 않도록 막음.(현재 요소에 해당된 이벤트까지만 동작)
	- event.stopImmediatePropagation() : 다른 이벤트 핸들러 및 현재 요소에 연결된 이벤트 핸들러도 동작하지 않게 막음.
	

## 18.11 Ajax
- 비동기적 자바스크립트 + XML의 약어
- 서버와 비동기적으로 통신하면 페이지 전체를 새로 고칠 필요 없이 서버에서 데이터를 받아 올 수 있음

~~~ javascript
	function refreshServerInfo(){
		const req = new XMLHttpRequest();
		req.addEventListener('load', function(){
			const data = JSON.parse(this.responseText);
			
			const serverInfo = document.querySelector('.serverInfo');
			
			Object.keys(data).forEach(p => {
				const replacements = serverInfo.querySelectorAll(`[data-replace="${p}"]`);
				for(let r of replacements){
					r.textContent = data[p];
				}
			});
		});
		req.open('GET', 'http://localhost:7070' , true);
		req.send();
	}
	refreshServerInfo();
~~~

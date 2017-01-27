---
layout: post
title:  "Property binding"
date:   2017-01-16 00:00:00
categories: TheTemplatingSyntax
comments: true
---

보간법은 템플릿에서 동적인 부분을 갖는 유일한 방법 중 하나이다. <br/>
실제로 우리가 보았던 보간법은 Angular 2 템플릿 시스템의 핵심 인 속성 바인딩을 사용하는 쉬운 방법이다. <br/><br/>

Angular 2에서는 각 대괄호를 대괄호 []로 묶인 HTML 요소의 특수 속성을 통해 작성할 수 있다. 
처음에는 이상하게 보이지만 실제로는 유효한 HTML 이다. 
HTML 속성은 따옴표, 어포 스트로피, 슬래시, 같음, 공백과 같은 몇 가지 문자를 제외하고 원하는 모든 것으로 시작할 수 있다. <br/><br/>

나는 DOM 속성에 대해 이야기하고 있지만 어쩌면 이것이 당신에게 명확하지 않을 수도 있다. 
우리는 일반적으로 HTML 속성에 쓰고 있다. <br/><br/>

```javascript
<input type="text" value="hello">
```

위의 입력 태그에는 두 가지 속성, 즉 유형 속성과 값 속성이 있다. 
브라우저가이 태그를 파싱하면 일치하는 속성 유형 및 값이있는 해당 DOM 노드 (HTMLInputElement)를 만든다.
각 표준 HTML 속성에는 DOM 노드에 해당 속성이 있다. 
그러나 DOM 노드에는 해당 특성이없는 추가 속성도 있다. (예 : childElementCount, innerHTML 또는 textContent )

사용자 이름을 표시하기 위한 보간법은 다음과 같다. 

```javascript
<p>{{user.name}}</p>
```

다음은 간단한 구문이다. 

```javascript
<p [textContent]="user.name"></p>
```

대괄호 구문을 사용하면 DOM 속성 textContent를 수정할 수 있으며 보간 용으로 사용 된 것처럼 현재 구성 요소 인스턴스의 컨텍스트에서 평가되는 user.name 값을 지정한다. 
구문 분석기는 대소문자를 인지하기 때문에 정확한 이름을 사용해야한다. 
DOM 속성은 HTML 속성보다 큰 이점을 가지고 있다. 만약 내가 입력 예제에서 value 속성은 항상 'hello'를 보여주는 반면에
DOM 의 Value 속성 노드는 브라우저에 의해 동적으로 수정되므로 사용자가 텍스트 필드에 입력 한 내용이 모두 포합된다. <br/><br/>

마지막으로 속성은 부울 값을 가질 수 있지만 일부 속성은 시작 태그에 존재하거나 부재함으로써 속성을 반영 할 수 있다. <br/>
예를 들어, <option> 태그에 selected 속성이 있다. <br/>
어떤 값을 지정했는지에 상관없이 옵션이있는 한 옵션을 선택한다. <br/><br/>

```javascript
<option selected>Rainbow Dash</option>
<option selected="false">Rainbow Dash</option> <!-- still selected -->
```

Angular 2 와 같은 속성 접근을 사용하게 되면

```javascript
<option [selected]="isPonySelected" value="Rainbow Dash">Rainbow Dash</option>
```

pony는 isPonySelected가 true이면 선택되어 질 것이고 그렇지 않으면 선택되지 않을 것이다. 
그리고 isPonySelected의 값이 변할 때마다 선택된 속성도 변경될 것이다. <br/>

Angular 1에서는 번거롭지만 많은 것들을 할수 있었다. 예를 들어 이미지를 동적 주소로 바인딩 하는것이 있다. 

```javascript
<img src="{{pony.avatar.url}}">
```
이 구문은 큰 문제가 있다. 브라우져는 src 속성을 읽자마자 이미지를 가져오려고 시도할 것이고 
실패로 보여질 것이다. 왜냐하면 {{pony.avatar.url}}은 유효한 URL이 아닌 HTTP 요청이기 때문이다. <br/>

그래서 Angular 1에서는 특별한 디렉티브가 있었다. : ng-src
```javascript
<img ng-src="{{pony.avatar.url}}">
```

src 대신 ng-src를 사용하여 문제는 해결하였다. 
AngularJS가 앱을 컴파일하고 나면 src 속성에 올바른 URL이 추가되어 이미지 다운로드가 시작된다. 
그러나 이는 두 가지 문제점이 있다. 

- 먼저 개발자로서 ng-src에 제공 할 가치를 알아야 한다. 그것은 'https://gravatar.com'이었습니까? ''https://gravatar.com ''? 'pony.avatar.url'? '{{pony.avatar.url}}'? 문서를 읽는 것 외에는 알 길이 없다. 
-  Angular 팀은 각 표준 속성에 대한 지침을 작성해야했습니다. 
그들은 그렇게했고, 우리는 그것들을 배워야했다. 
그러나 이제는 HTML에 외부 웹 구성 요소가 포함될 수있는 세상에 있다. 

```javascript
<ns-pony name="Rainbow Dash"></ns-pony>
```

Web Component를 사용하려는 경우 Web Component 개발자가 주의 깊게 처리 한 경우를 제외하고 대부분의 JS 프레임 워크에서 동적 값을 전달하는 쉬운 방법이 없다. 
자세한 내용은 Web Component 챕터를 읽어보도록 해라. <br/>

Web Component는 브라우져 엘리먼트처럼 동작해야 한다. 
Web Component들은 Angular 2기반의 속성, 이벤트, 함수 등을 가져야 한다.  

```javascript
<ns-pony [name]="pony.name"></ns-pony>
```

그리고 정상적으로 동작한다. 
Angular는 속성과 요소에 대한 동기화를 유지할것이다. 
더 이상 지시어를 배울 필요가 없다. 요소를 숨기려면 표준 숨김 속성을 사용할 수 있다. 

```javascript
<div [hidden]="isHidden">Hidden or not</div>
```

ishidden이 true일 경우 div는 사라질 것이다. Angular는 숨겨진 속성과 직접 작동하기 때문이다. 
더 이상 Angular에서 사용된 directives 중의 하나 인 ng-hide와 같은 directive를 사용할 필요가 없다. 
스타일 속성의 color 속성과 같이 중첩된 속성에 접근할 수도 있다. 

```javascript
<p [style.color]="foreground">Friendship is Magic</p>
```

foreground의 속성이 '녹색'으로 변한다고 하면, color도 녹색으로 변할 것이다. <br/>
따라서 Angular 2는 속성을 사용하고 있다. 어떤 값을 전달할 수 있을까? 
우리는 이미 보간 속성 property="{{expression}}" 을 이미 보았다. 

```javascript
<ns-pony name="{{pony.name}}"></ns-pony>
```

이것은 [property] ="expression"과 동일하다. 

```javascript
<ns-pony [name]="pony.name"></ns-pony>
```

'Pony' 글자 뒤에 pony의 이름을 덧 붙이기를 원한다면 당신은 두 가지 옵션이 있다. 

 ```javascript
<ns-pony name="Pony {{pony.name}}"></ns-pony>
<ns-pony [name]="'Pony ' + pony.name"></ns-pony>
```

만약 당신이 동적인 변수를 사용하지 않는다면 property = "value" 와 같이 간단하게 적을 수 있다. 

 ```javascript
 name="Rainbow Dash"></ns-pony>
```

이들 모두는 동등하고 구문은 개발자가 구성 요소를 설계하는 방법에 달려 있지 않는다.  
구성 요소가 값 또는 참조 예를 기대하고 있는지를 알아야하는 AngularJS 1.x의 경우와 같다고 할 수 있다. 
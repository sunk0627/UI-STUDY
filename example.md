## CSS NEW FEATURES 2023
 container, layer, viewport 단위, :has(), overscroll behavior, subgrid, accent color, media query의 범위


### 1. CONTAINER QUERY :
브라우저 viewport 기준이 아닌 컨테이너 사이즈에 따라 변경가능하다.
  
```css
.parent {
  container-type: inline-size;
  container-name: parent;       /* 쿼리 컨테이너 이름 지정 */
}    
@container parent (min-width: 300px) { /* 쿼리 컨테이너 이름 사용 */
  .child {
   /* styling */
  }
}
```
예 ) <br>
<http://styleship.com/ui/html/container.html> <br>
- 아래 예시에서는 버튼을 클릭 시 부모 컨테이너의 사이즈에 따라 폰트크기가 달라지는 걸 볼 수 있다.
```css
/* example */
.container {
    width: 1400px;
    margin: 0 auto;
}
#btn1 {
    position: absolute;
    text-indent: -9999;
    opacity: 0;
}
#btn1 + label {
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100px;
    height: 50px;
    font-size: 20px;
    font-weight: 700;
    color: aliceblue;
    background-color: violet;
}
.container .sidebar {
    padding: 20px;
    width: 100px;
    height: 300px;
    border: 1px solid #000;
    box-sizing: border-box;
    container-name: container;
    container-type: inline-size;
}
.container .sidebar p {
    text-align: center;
    font-size: 18px;
}
#btn1:checked ~ .sidebar {width: 200px;}

@container container (min-width: 100px) {
    .container .sidebar p {
        font-size: 28px;
        font-weight: 700;
    }
}
```
```html
<div class="wrap">
    <div class="container">
        <input type="checkbox" id="btn1">
        <label for="btn1">BUTTON</label>
        <div class="sidebar">
            <p>sidebar</p>
        </div>
    </div>
</div>
```

- container-name 
  - 컨테이너의 이름을 지정한다.
  - 지정된 이름은 @container **컨테이너이름** 형식으로 쓰인다.
- container-type
  - inline-size: 인라인 레벨 기준으로 컨테이너 적용 (주로 쓰인다)
  - size: 블록 레벨 기준으로 컨테이너 적용
  - normal: none의 의미
-  IE 제외한 나머지 브라우저 호환.


### 2. VIEWPORT 단위 (svh, lvh, dvh) : 
모바일 safari에서 vh를 결정할 때 일부 UI를 무시하는 버그가 있다. <br>height: 100vh로 지정시 페이지가 로드됐을 때 높이가 화면 넘치게 지정되는 현상이 발생한다. <br>
vh 버그를 해결하기 위한 방법으로 새로운 뷰포트 단위를 사용하면 모바일 브라우저 사용 환경에 알맞은 화면의 높이값을 반영할 수 있다.<br>
예) 

<http://styleship.com/ui/html/vh_test.html>
*(모바일 브라우저로 확인)*
   - **svh** (short viewport height)
     -  사용자가 볼 수 있는 가장 작은 뷰포트 높이를 반영한다.
     -  viewport 높이에서 모든 인터페이스 요소를 제거한 높이라고 할 수 있다.
   - **lvh** (large viewport height) 
     - 사용자가 볼 수 있는 가장 큰 뷰포트 높이를 반영한다.
     - viewport 높이에서 모든 인터페이스 요소를 포함한 높이라고 할 수 있다.
   - **dvh** (dynamic viewport height) 
     - 현재 뷰포트 높이를 나타내는 동적인 값이다.
     - 만약 주소표시줄이 축소된 상태에서 스크롤을 통해 주소표시줄이 확장되면 dvh의 값도 업데이트된다.
     - dvh를 사용하면 정확히 viewport에 맞게 콘텐츠의 크기를 조정할 수 있다.
  <br><br>
  - safari 15.4 이상/ 크롬 지원
  
### 3. :has() : 
해당 자식을 갖고 있는 부모 요소를 선택하여 적용하는 가상 클래스이다. <br>
css의 if문 버전이라고 생각하면 된다. 
- 파이어폭스/익스를 제외한 브라우저에서 지원된다.
```html
<!-- example -->
<div class="parent">
  <p>Child</p>
</div>
```
```css
/* div 요소가 자식으로 p를 가지고 있을 경우 스타일 적용 */
div:has(> p) {
  background: red;
}
```
- :has() 선택자 조합 종류
```css
/* a 요소가 자손으로 img를 가지고 있을 경우 */
a:has(img) {
}

/* a 요소가 자식으로 img를 가지고 있을 경우 */
a:has(> img) {
}

/* h3 요소가 형제로 p를 가지고 있을 경우 */
h3:has(+ p) {
}

/* article 요소가 자손으로 h3 그리고 p를 가지고 있을 경우 (and 논리) */
article:has(h3, p) {
}
```
- :has()와 :not()의 조합
```css
/* div 요소가 자손으로 h3을 가지고 있지 않을 경우 전체 칼라 적용 */
div.section1:not(:has(h3)) {
  color: red;
}
```
  
예 ) <br> <http://styleship.com/ui/html/has_test.html>
- 아래 예시에서는 부모 image-gallery박스가 마우스오버 시의 img를 가지고 있을 경우 마우스 오버하지 않은 img의 scale 작게, opacity 흐리게 스타일을 적용한 것이다.
```css
/* example */
.has-img-wrap {
    padding: 50px;
    box-sizing: border-box;
}
.has-img-wrap h2 {
    text-align: center;
    padding-bottom: 40px;
    color: pink;
}

.has-img-wrap .image-gallery {
    margin: 0 auto;
    width: 900px;
    display: flex;
    flex-wrap: wrap;
}

.has-img-wrap .image-gallery img {
    object-fit: cover;
    width: 300px;
    height: 300px;
    /* scale: .8;
    opacity: .7; */
    transition: all ease .3s;
}
/* 마우스오버시의 img를 가지고 있는 .image-gallery 박스 아래 img */
.has-img-wrap .image-gallery:has(img:hover) img:not(:hover) {
    scale: .8;
    opacity: .7;
}

/* .image-gallery img:hover {
    scale: 1;
    opacity: 1;
} */
```
```html
<div class="has-img-wrap">
    <h2>image gallery</h2>
    <div class="image-gallery">
        <img src="/ui/images/_temp/1.png" alt="">
        <img src="/ui/images/_temp/2.png" alt="">
        <img src="/ui/images/_temp/3.png" alt="">
        <img src="/ui/images/_temp/1.png" alt="">
        <img src="/ui/images/_temp/2.png" alt="">
        <img src="/ui/images/_temp/3.png" alt="">
        <img src="/ui/images/_temp/1.png" alt="">
        <img src="/ui/images/_temp/2.png" alt="">
        <img src="/ui/images/_temp/3.png" alt="">
    </div>
</div>
```
### 4. OVERSCROLL BEHAVIOR :
우리가 스크롤에서 겪을 수 있는 이슈 중 하나가 스크롤 체이닝 현상이다. <br>
>*스크롤 체이닝(scroll chaining) 이란? 하위 컨테이너박스의 스크롤이 맨 하단에 도달했을 때 상위 컨테이너박스의 스크롤도 같이 움직이는 현상이다.<br>

overscroll-bahavior:contain 속성은 이러한 상황을 방지할 수 있게 해준다.<br>

예 )<br>
<http://styleship.com/ui/html/overscroll.html> 
- 아래 예시에서는 overscroll-behavior의 속성값인 auto와 contain을 비교하였다.
- 모든 브라우저 호환
```css
.scroll-wrap {
	display: flex;
	justify-content: center;
	gap: 2rem;
	padding: 100px;
	min-height: 200vh;
	background-color: #ffc857;
	box-sizing: border-box;
}
.scroll-box {
 width: 200px;
}
.scroll-box ul {
	position: relative;
	padding: 0;
	margin: 0;
	height: 50vh;
	overflow: auto;
	list-style: none;
}
.no-scroll-chain {
	overscroll-behavior: contain;
}
h2 {
	padding: 10px;
	font-size: 14px;
	font-weight: 700;
	text-align: center;
	color: skyblue;
	background-color: #777;
}
.scroll-box ul li {
	background-color: rgb(255,255,255);
	padding: 10px 20px;
	box-sizing: border-box;
}
.scroll-box ul li:nth-child(even) {
	background-color: #eee;
}
```
```html
<div class="scroll-wrap">
    <div class="scroll-box">
        <h2>auto</h2>
        <ul>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
        </ul>
    </div>
    <div class="scroll-box">
        <h2>contain</h2>
        <ul class="no-scroll-chain">
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
            <li>source</li>
        </ul>
    </div>

</div>
```
### 5. ACCENT COLOR :
input에 색상을 지정해줄 수 있다. <br>
- 브라우저에서 자동으로 색 대비를 고려하여 최적화 해준다.
-  IE 제외한 나머지 브라우저 호환.

예 ) <br>
<http://styleship.com/ui/html/accent_color.html>
```html
<!-- example -->
<div class="accent-color-box">
    <div class="inp-box">
        <input type="radio" id="1" name="radio" checked>
        <label for="1">1</label>
        <input type="radio" id="2" name="radio">
        <label for="2">2</label>
        <input type="radio" id="3" name="radio">
        <label for="3">3</label>
    </div>
    <div class="inp-box">
        <input type="checkbox" id="4" checked>
        <label for="4">4</label>
        <input type="checkbox" id="5">
        <label for="5">5</label>
    </div>
    <div class="inp-box2">
        <input type="checkbox" id="4" checked>
        <label for="4">4</label>
        <input type="checkbox" id="5">
        <label for="5">5</label>
    </div>
</div>
```
```css
.accent-color-box {
    padding: 30px;
    text-align: center;
    box-sizing: border-box;
}

.accent-color-box input[type="radio"] {
    accent-color: deeppink;
    width: 20px;
    height: 20px;
    vertical-align: middle;
}

.accent-color-box input[type="radio"]+label {
    font-size: 18px;
    vertical-align: middle;
}

.accent-color-box input[type="checkbox"] {
    accent-color: orange;
    width: 20px;
    height: 20px;
    vertical-align: middle;
}

.accent-color-box input[type="checkbox"]+label {
    font-size: 20px;
    vertical-align: middle;
    color: cadetblue;
}

.accent-color-box .inp-box2 input[type="checkbox"] {
    accent-color: deeppink;
    width: 20px;
    height: 20px;
    vertical-align: middle;
}

.accent-color-box .inp-box2 input[type="checkbox"]+label {
    font-size: 20px;
    vertical-align: middle;
    color: cadetblue;
}
```
### 6. MEDIA QUERY RANGES :
미디어 쿼리의 최대 / 최소 너비 지정이 간편해졌다.
- IE를 제외한 모든 브라우저 호환
```css
/* Classic Way */
@media only screen (min-width: 300px) and (max-width: 750px) {

}

/* Rewritten using a Range Context */
@media only screen (300px <= width <= 750px) {

}
```
### 7. INERT 
HTML 요소에 inert 속성을 사용하면 해당 요소를 비활성 상태로 만들 수 있다.
- pointer-events: none의 역할을 한다.
- user-select: none의 역할을 한다.
- IE 를 제외한 모든 브라우저 호환
  
```html
<div class="box1" inert>비활성</div>
<div class="box2">활성</div>
<a href="" inert>비활성</a>
<a href="">활성</a>
<input type="checkbox" inert>	
<input type="text" inert>
<input type="text">
<hr>
<button inert>비활성</button>
<button>활성</button>
```
```css
[inert] {
    opacity: 0.5;
}
```
```js
//이벤트
$('.box1').click(function(){
    alert('box1 clicked');
});
$('.box2').click(function(){
    alert('box2 clicked');
});

//스타일
$('.box1').css("color", "red");
$('.box2').css("color", "red");
```
- 이점
    - 화면 밖에 숨겨져 있다가 나타나는 ui를 접했을 때 활용할 수 있다. (lnb영역이 보였다가 닫히면 표시되지 않는 서랍형 ui)
    - lnb가 닫혀있는 상태에서 탭하여 포커스를 이동한 후에 인풋창에 타이핑을 하면 콘솔에 글씨가 찍힌다. lnb가 닫힌 경우에는 input에 inert속성을 추가하여 타이핑 되는 것을 막아준다.

예) <br>
http://styleship.com/ui/html/inert.html

### 8. CASCADE LAYERS :
css 스타일을 선언할 때 우선순위를 결정하는 방식들이 있는데 그 중의 하나로 @layer가 추가되었다. <br>
예를들면, 아래와 같이 스타일을 레이어로 그룹화할 수 있으며 레이어의 스타일 우선순위를 정할 수 있다.<br>

예) <br>
http://styleship.com/ui/html/cascade_layer.html
```html
<p class="text">hello world</p>
```
```css
@layer style {
    .text {
        color: red;
    }
}
```
- 레이어는 선언 순서에 따라 스타일이 적용되며, 늦게 선언될수록 높은 우선순위를 가지게 된다.<br>
아래 코드에서는 layer_3 > layer_2 > layer_1 순으로 스타일 우선순위를 가진다.
```css
@layer layer_1 {
  .text {
    color: red;
    font-weight: 700;
  }
}

@layer layer_2 {
  .text {
    color: blue;
    font-weight: 400;
  }
}

@layer layer_3 {
  .text {
    color: orange;
  }
}
```
- 레이어 이름을 재사용 할 수 있다. 
    - 이미 선언한 레이어에 스타일을 추가하고 싶을 때 레이어 이름을 통해 추가할 수 있다.
    - 레이어의 스타일을 추가한다고 해서 레이어의 우선순위가 변하지 않는다.
```css
/*  layer_1에 스타일 추가 */
@layer layer_1 {
  .text {
    font-style: italic;
  }
}
```
- revert-layer로 레이어 스타일을 되돌릴 수 있다.
```css
@layer base {
  p {
    color: red;
  }
}

@layer next {
  p {
    color: blue;
  }

  /* p태그에 text클래스가 지정되어 있으면, color가 이전 레이어(base)의 red로 적용됨 */
  .text {
    color: revert-layer;
  }
}
```

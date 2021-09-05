# 프로젝트 생성
- npm init -y
- npm install -D parcel-bundler
- package.json 파일의 script 부분에 아래내용으로 변경.
    - "dev": "parcel index.html",
    - "build": "parcel build index.html"

# 주석
```scss
$color: royalblue;

.container {
    h1 {
        color: $color;
        /* background-color: orange; */ 
        // font-size: 60px; 
    }
}
```
주석 | 설명
-- | --
/* */ | css 코드에 들어감.
// | css 코드에 들어가지 않음.

# 중첩
- html
```html
<div class="container">
    <ul>
        <li>
            <div class="name">SJ</div>
            <div class="age">22</div>
        </li>
    </ul>
</div>
```
- css
```css
.container ul li {
    font-size: 40px;
}
.container ul li .name {
    color: red;
}
.container ul li .age {
    color: orange;
}
```
- scss
```scss
.container {
    ul {
        li {
            font-size: 40px;
            .name {
                color: red;
            }
            .age {
                color: orange;
            }
        }
    }
}
```

# 상위 선택자 참조 - &
- css
```css
.btn {
    position: absolute;
}
.btn .active {
    color: red;
}

.list li:last-child {
    margin-right: 0;
}
```

- scss
```scss
.btn {
    position: absolute;
    &.active {
        color: red;
    }
}
.list {
    li {
        &:last-child {
            margin-right: 0;
        }
    }
}
```

- css
```css
.fs-small {
    font-size: 12px;
}
.fs-medium {
    font-size: 14px;
}
.fs-large {
    font-size: 16px;
}
```

- scss
```scss
.fs {
    &-small { font-size: 12px; }
    &-medium { font-size: 14px; }
    &-large { font-size: 16px; }
}
```

# 중첩된 속성
- css
```css
.box {
    font-weight:bold;
    font-size: 12px;
    margin-top: 10px;
    margin-bottom: 20px;
    padding-top: 10px;
    padding-bottom: 10px;
}
```
- scss
```scss
.box {
    font: {
        weight:bold;
        size: 12px;
    };
    margin: {
        top: 10px;
        bottom: 20px;
    };
    padding: {
        top: 10px;
        bottom: 10px;
    };
}
```

# 변수
- css
```css
.container {
    position: fixed;
    top: 200px;
}
.container .item {
    width: 200px;
    height: 200px;
    transform: translateX(200px);
}
```
- scss
```scss
.container {
    $size: 200px;
    position: fixed;
    top: $size;
    .item {
        $size: 100px;
        width: $size; // 100px
        height: $size; // 100px
        transform: translateX($size); // 100px
    }
    .left: $size; // 100px
}

.box {
    width: $size; // error
}
```

# 산술 연산
```scss
div {
    width: 20px + 20px; // 40px
    height: 40px - 10px; // 30px
    font-size: 10px * 2; // 20px
    margin: 30px / 2; // 30px / 2
    padding: 20px % 7; // 6px 
}

span {
    font-size: 10px;
    line-height: 10px;
    font-family: serif;

    // 위의 3가지를 종합해서 작성한 것. 그래서 나누기 연산이 적용되지 않음.
    font: 10px / 10px serif; 
}
```

- 나누기 연산을 하는 방법
```scss
div {
    $size: 30px;


    margin: (30px / 2);
    margin: $size / 2;
    margin: (15px + 15px) / 2;
}
```

- 기본적인 단위는 동일해야함.
- 단위가 다를 경우 calc()를 사용해야함.

# 재활용(Mixins)
- css
```css
.container {
    display: flex;
    justify-content: center;
    align-items: center;
}

.container .item {
    display: flex;
    justify-content: center;
    align-items: center;
}

.box {
    display: flex;
    justify-content: center;
    align-items: center;
}
```
- scss
```scss
@mixin center {
    display: flex;
    justify-content: center;
    align-items: center;
}

.container {
    @include center;
    .item {
        @include center;
    }
}

.box {
    @include center;
}
```

- css
```css
.container {
    width: 100px;
    height: 100px;
    background-color: red;
}

.container .item {
    width: 200px;
    height: 200px;
    background-color: orange;
}

.box {
    width: 200px;
    height: 200px;
    background-color: orange;
}
```

- scss
```scss
@mixin box($size: 200px, $color: orange) { // 인수 사용
    width: $size;
    height: $size;
    background-color: $color;
}

.container {
    @include box(100px, red);
    .item {
        @include box($color: green); // 키워드 인수
    }
}

.box {
    @include box;
}
```
- @mixin은 css코드의 모음을 하나의 이름으로 만들어서 그것을 재활용하는 용도로 사용함.
- js의 함수와 유사함.

# 반복문
- 보간 
    언어 | 방식
    -- | --
    js | ${x}
    scss | #{x}

- js 반복문
```js
for(let i = 0; i < 10; i+= 1) {
    console.log(`loop-${i}`)
}
```
- scss 반복문
```scss
@for $i from 1 through 10 {
    .box:nth-child(#{$i}) {
        width: 100px * $i;
    }
}
```

# 함수
- css

```css
.box {
    width: 100px;
    height: 50px;
    display: flex;
    justify-content: center;
    align-items: center;
}
```

- scss
```scss
@mixin center {
    display: flex;
    justify-content: center;
    align-items: center;
}

@function ratio($size, $ratio) {
    @return $size * $ratio
}

.box {
    $width: 100px;
    width: $width;
    height: ratio($width, 1/2);
    @include center;
}
```
- js 함수와 동일.
- @mixin은 우리가 알고있는 코드의 모음 정도.
- @function은 실제로 어떤 값을 따로 연산을 해서 반환된 결과로 사용.

# 색상 내장함수
내장함수 | 설명
-- | --
mix(컬러1, 컬러2) | 컬러1과 컬러2의 혼합된 색상.
lighten(컬러, x%) | 해당 %만큼 컬러를 밝아지게 함.
darken(컬러, x%) | 해당 %만큼 컬러를 어두워지게 함.
saturate(컬러, x%) | 해당 %만큼 컬러의 채도를 올림.
desaturate(컬러, x%) | 해당 %만큼 컬러의 채도를 내림.
grayscale(컬러) | 컬러를 회색으로 만들어줌.
invert(컬러) | 컬러를 반전시키는 효과.
rgba(컬러, 투명도) | 컬러의 투명도를 설정.

# 가져오기
```scss
// @import url("./sub.scss");
// @import "./sub.scss";
@import "./sub", "./sub2";
```

# Overwatch 캐릭터 선택 예제 리팩토링
리팩토링은 '결과의 변경 없이 코드의 구조를 재조정함'을 뜻 함.<br />
<h2><a href="https://github.com/Seong-Jun1525/overwatch">완성본 바로가기</a></h2>

# 데이터 종류
- css
```css
.box {
    width: 100px;
    color: red;
}
```
- scss
```scss
$number: 1; // .5, 100px, lem
$string: bold; // relative, "../img/a.png"
$color: red; // blue, #fff, rgba(0,0,0,.1)
$boolean: true; // false
$null: null;
$list: orange, royalblue, yellow;
$map (
    o: orange,
    r: royalblue,
    y: yellow
);

.box {
    width: 100px;
    color: red;
    position: null;
}
```

# 반복문 @each
- scss
```scss
$number: 1; // .5, 100px, lem
$string: bold; // relative, "../img/a.png"
$color: red; // blue, #fff, rgba(0,0,0,.1)
$boolean: true; // false
$null: null;
$list: orange, royalblue, yellow;
$map (
    o: orange,
    r: royalblue,
    y: yellow
);
```

```scss
// @each 키워드를 통해서 $list 변수에 있는 해당 데이터들을 반복적으로 $c 변수에 담아서 {}에서 처리하겠다는 의미.
@each $c in $list {
    .box{
        color: $c;
    }
}
```
```css
.box {
    color: orange;
}
.box {
    color: royalblue;
}
.box {
    color: yellow;
}
```
```scss
@each $key, $value in $map {
    .box-#{$key} {
        color: $value;
    }
}
```
```css
.box-o {
    color: orange;
}
.box-r {
    color: royalblue;
}
.box-y {
    color: yellow;
}
```

# 재활용 @content
```scss
@mixin left-top {
    position: absolute;
    top: 0;
    left: 0;
    @content;
}
.container {
    width: 100px;
    height: 100px;
    @include left-top;
}
.box {
    width:200px;
    height: 300px;
    @include left-top {
        bottom: 0;
        right: 0;
        margin: auto;
    }
}
```

- @include left-top { ... } 에 추가된 속성을 content부분에 넣어줌.


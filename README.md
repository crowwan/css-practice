# 부스트코스 HTML/CSS 활용

## <CSS 리셋>
브라우저에서 기본적으로 제공하는 속성들을 초기화 해준다. 각 브라우저마다 다른 값을 제공하게 되면 크로스브라우징 원칙을 지키지 못하게 된다.

리셋 CSS는 정해진 형식이나 코드가 없다.
사용하지 않는 태그도 초기화할 필요가 없고, 프로젝트 성격에 맞춰 작성하면 된다.

```css
body,
h1,h2,h3,h4,h5,h6,
ul,ol,dl,dd,p,
fieldset,legend{
  margin: 0;
  padding: 0;
}
ul,ol{
  list-style: none;
}
table{
  border-collapse: collapse;
}
em{
  font-style: normal;
}
a{
  color: inherit;
  text-decoration: none;
}
a:hover{
  text-decoration: underline;
}
img{
  vertical-align: top;
}

fieldset{
  border:0;
}
```

- 기본적으로 들어가면 좋은 것은 블록형태의 태그의 margin과 padding을 0으로 초기화시키는 것이다.
- 리스트의 경우 기본으로 제공되는 스타일은 안 쓰는 경우가 많기 때문에 `list-style: none;`으로 초기화한다.
- 테이블의 경우 외곽선이 분리되어 있으므로 보통의 경우 `collapse`를 통해 하나로 합쳐준다.
- img태그는 사진이 전체 영역을 차지하지 못하고 빈공간이 남아있는데 `vertical-align: top;`을 통해 그 공간을 없애준다.

---

## IR(Image Replacement)
웹 접근성은 **모든 사용자가 모든 기기에서 웹에 접근할 수 있도록 하는 것**이다.

기본적으로 적절한 대체 텍스트를 제공해야 하는데 IR은 대체 텍스트를 위한 것이다. 

### - 사용하는 경우

<img> 태그의 alt 속성 값으로 표현하기에 대체 텍스트가 너무 길거나, CSS background 속성을 사용하여 처리한 의미 있는 이미지일 경우, 마크업으로 대체 텍스트를 제공한다.

### - 방법

마크업을 숨기는 방식이다. 많은 방법이 있지만, 좋은 방법은 숨김이 필요한 태그에 클래스를 두고 그 클래스에 따로 css처리를 통해 숨김 처리를 한다.

``` html
<span class="blind">숨김 텍스트</span>
```
``` css
.blind {
    /* 레이아웃에 영향을 끼치지 않도록 */
    position: absolute;

    /* 스크린 리더가 읽을 수 있도록 */
    width: 1px;
    height: 1px;

    /* 눈에 보이는 부분을 제거 */
    clip: rect(0 0 0 0);
    margin: -1px;
    overflow: hidden;
}
```
- 숨김 처리가 필요한 모든 태그의 클래스에 blind를 추가하면 된다.
- 숨김 처리를 한 요소는 스크린 리더가 읽을 수 있어야 한다.

웹 접근성에 대해서는 따로 공부를 더 해볼 생각이다.

---
## 레이아웃

웹 사이트를 만드는 데 있어 가장 기본이 되는 것은 레이아웃이다. 건축에 도안과 같다고 생각하면 된다.

과거 레이아웃을 `<table>`을 통해 만들기도 했지만 웹 표준이 발표되면서 현재는 `<table>`을 사용해서 레이아웃을 제작하지 않는다.

이전에 레이아웃을 만들 때는 `display: flex`를 이용했지만, 이 강의에서는 `float`과 `display: table`을 이용하여 간단한 레이아웃을 만들었다. 잘 사용하지 않았던 것들이라 도움이 된 것 같다.

### 1단 레이아웃
```css
.wrap{
      min-width: 800px;
      text-align: center;
    }
header{
      height: 100px;
      background: lightgreen;
    }
.content{
      max-width: 1200px;
      height: 300px;
      margin: 0 auto;
      background-color: lightsalmon;
    }
footer{
      height: 100px;
      background-color: lightblue;
    }
```
단순한 레이아웃 형태로 header와 content, footer로 구성되어 있다. 
- `.content`를 중앙으로 위치시키기 위해 `margin: 0 auto;`를 사용했다.
- `.content`의 최대 너비와 최소 너비를 각각 1200px, 800px로 주었는데, 최소 너비의 경우 `.content`에만 적용하게 되면 800px보다 창이 작아질 때 `header`와 `footer`는 줄고 `.content`는 800px에서 멈춰있어 다 감싸고 있는 `.wrap`에 최소 너비를 설정했다.

### 2단 레이아웃

```css
.container{
        /* position: relative; */
        display: table;
        table-layout: fixed;
        width: 800px;
        min-height: 100%;
        margin: -100px auto;
        padding:100px 0;
        box-sizing: border-box;
      }
.content{
        /* float: left; 
        height: 300px;*/
        display: table-cell;
        width: 500px;
        background-color: lightsalmon;
        border-right: 5px solid #000;
      }
aside{
        display: table-cell;
        /* float:left;
        height: 300px; */
        width: 300px;
        background-color: lightseagreen;
      }
```
1단 레이아웃에서 `aside`가 추가된 형태이다.

- `float`과 `display: table` 중 하나를 이용하여 구현가능하다. 내 경우 후자가 더 편한 것 같다.
- `float`을 이용할 경우 `footer`가 옆에 붙기 때문에 `::after`에 `clear: both;`를 줘서 해제해야 한다.

### 고정 레이아웃
```css
header{
      position: fixed;
      top: 0; right: 0; left: 0;
      height: 100px;
      background: lightgreen;
    }
.container{
      min-height: 100%;
      margin-top: -100px;
      /* 
      padding-top: 200px;
      box-sizing: border-box;
      */
    }
.content{
      max-width: 1200px;
      height: 300px;
      margin: 0 auto;
      background-color: lightsalmon;
      padding-top: 200px;
    }
```
많은 웹 사이트에 고정 레이아웃이 들어간 것을 알 수 있다. 꼭 헤더만이 아니다.
- `position: fixed;`를 통해 고정시킬 수 있고, 고정되는 위치는 따로 지정해줘야 한다.
- `.container`에 최소 높이를 100%로 지정했으므로 `footer`가 화면 밑에 있게 된다. 이를 해결하기 위해 `margin-top: -100px;`을 하면 `.content`의 내용이 `header`에 가려지게 되므로, 음수 마진과 헤더의 높이를 더한 200px을 `padding`으로 갖으면 모든 요소가 화면에 보이게 된다.
- 이때 `.container`에 `padding`을 주면 최소 높이에 패딩이 더해지기 때문에 `box-sizing: border-box`를 설정해야 한다. `.content`에 추가하는 경우에는 `.container`의 안의 내용에 패딩이 생기므로 상관이 없다. **하지만 명백히 둘의 차이는 있다고 생각한다.**
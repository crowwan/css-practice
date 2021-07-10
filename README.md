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

---

## 메뉴
### 1단 메뉴
```css
.menu{
  display: table;
  width: 100%;
  table-layout: fixed;
}
.menu_item{
  display: table-cell;
}
.menu_link{
  display: block;
  height: 36px;
  border: 1px solid #ddd;
  font-size: 12px;
  line-height: 36px;
  color: #333;
  text-align: center;
}
.menu_item + .menu_item .menu_link{
  margin-left: -1px;
  /* 첫 메뉴 이후의 메뉴들의 왼쪽 외곽선을 -1px을 통해 겹친다. */
}
.menu_item:hover .menu_link{
  color: green;
  font-weight: bold;
}
.menu_item.active .menu_link{
  position: relative; 
  /* 
  위에서 margin-left를 통해 옆 메뉴의 외곽선이 겹쳐지는데,
  active된 메뉴의 외곽선 색이 달라야 하지만 겹쳐진 외곽선이 위로 올라오기 때문에
  position: relative로 올려 옆 메뉴의 외곽선을 가린다.
  */
  border-color: #555;
  font-weight: bold;
  color: #fff;
  background-color: gray;
}
```
- 메뉴를 만들때 `display: table`을 사용했는데 이때 외곽선이 겹쳐지지 않아 중간이 더 두꺼워 보이게 된다. 이를 해결하기 위해서 메뉴 칸의 인접선택자를 통해 `margin-left: 1px`을 했다.
- 위에서 외곽선을 겹쳤는데 이로 인해서 메뉴의 외곽선 색이 바뀌면 옆 메뉴의 외곽선이 더 위에 올라오게 되는 현상이 나타난다. 그렇기 때문에 메뉴 칸을 `position: relative`를 통해 띄워주어 옆 메뉴의 외곽선을 가린다.

### 2단메뉴
```css
.subMenu {
  display: none;
  position:absolute;
  left: 0;
  width: 100%;
  min-width: 700px;
  border-top: 1px solid #eee;
  border-bottom: 1px solid #eee;
}
.subMenu .subMenu_item{
  display: inline-block;
}
.subMenu_link{
  display: block;
  padding: 15px 35px;
  font-size: 17px;
  line-height: 20px;
  color: #333;
}
.subMenu_item:hover .subMenu_link,
.subMenu_item.active .subMenu_link
{
  font-weight: bold;
  color: green;
}
.subMenu_link span{
  position: relative;
}
.subMenu_item:hover span::after,
.subMenu_item.active span::after{
  position: absolute;
  content: '';
  right: 0;
  bottom: -15px;
  left: 0;
  border-bottom: 2px solid green;
}
```
- 2단 메뉴에서는 `display: table`을 사용하지 않았다. 
- 여기서 중요한 것은 서브 메뉴가 `.active`를 가진 메인메뉴인 경우에 보여야 하므로 기본적으로 `display: none;`으로 가려준다.
- 서브 메뉴가 `.active`되면 그 밑에 줄이 나타나는데 이는 앵커 테그의 `text-decoration`이 아니라 메뉴 내용을 `span`태그로 묶어 그것의 `::after`에 효과를 준다.

---

## 탭
``` css
.tab{
  display: table;
  table-layout: fixed;
}
.tab_item + .tab_item .tab_btn{
  width: 101px;
  margin-left: -1px;
}
.tab_item{
  display: table-cell;
}
.tab_btn{
  position: relative;
  width: 100px;
  height: 50px;
  font-size: 16px;
  color: #999;
  background-color: transparent;
  border: 1px solid #eee;
  outline: 0;
  cursor: pointer;
}
.panel_item .item_link{
  display: block;
  font-size: 14px;
  line-height: 30px;
  color: #333;
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
```
탭과 네비게이션의 차이점은 새로운 페이지가 열리느냐 아니냐이다. 탭의 경우 새로운 페이지가 열리지 않고 탭의 내용만 바뀌게 된다.

- `<a>`태그가 아닌 `button`태그를 사용했다.
- 내용 중 넘치는 것을 '...'으로 처리했다. 방법은 `overflow: hidden; white-space: nowrap; text-overflow: ellipsis;`를 넣어주면 된다.
---
## 이미지(썸네일)
```css
.main_list .item_link{
  position: relative;
  display: block;
}
```
- `img`태그를 감싸고 있는 앵커를 이미지 크기에 맞게 `display:block`시켜준다.

```css
.main_list .img_box{
  position: relative;
}
.main_list .img_box::after{
  position: absolute;
  top: 0; right: 0; left: 0; bottom: 0;
  content: '';
  background-color: rgba(0,0,0,.2);
  border: 1px solid rgba(0,0,0,.05);
}
```
- 이미지를 덮는 박스를 만들 때는 `::after`를 사용한다.
- 이때 `::after`요소가 이미지 크기와 같게 하기 위해서 상하좌우를 모두 0으로 준다.
```css
.rank .down::before{
  width: 7px;
  height: 10px;
  background: url(../img/rank_down.png) no-repeat;
}
```
- 아이콘을 넣을 때는 아이콘의 크기를 넣어주고 `background: url()`을 통해 넣어줄 수 있다. 이 경우 `.blind`를 가진 `span`을 통해 아이콘 설명을 숨김 처리해야 한다.
``` css
.sub_list .info_wrap{
  position: relative;
  margin-top: 10px;
}
.sub_list .watch_later_link{
  display: none;
  position: absolute;
  right: 4px;
  bottom: 4px;
  width: 45px;
  height: 45px;
  background: url(../img/later_watch.png) no-repeat;
}
.sub_list .img_wrap:hover .watch_later_link{
  display: block;
}
```
- 사진에 마우스를 올렸을 때 사진 위에 아이콘이 나오는 경우에는 사진에 `:hover`를 하는 것이 아닌 사진과 아이콘을 모두 감싸는 태그에 `:hover`를 한 경우에 보이게 해야지 아이콘에 올렸을 때 이상없이 작동한다.

```css
.sub_list .title{
  display: -webkit-box;
  max-height: 36px;
  text-overflow: ellipsis;
  overflow: hidden;
  -webkit-box-orient: vertical;
  -webkit-line-clamp: 2;
  font-size: 15px;
  line-height: 18px;
  color: #090909;
}
```
- 말줄임을 2줄 이상에서 하고 싶을 때는 위와 같이 할 수 있지만, `-web-kit`을 지원하지 않는 브라우저에서는 `text-overflow: ellipsis;`효과가 나타나지 않고, `max-height`를 반드시 지정해줘야 한다.
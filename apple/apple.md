# :pushpin: 마크업

:star: [apple.html 바로가기](/apple/apple.html)

![makup](/apple/products/00-makup-structure.png)<br/>

- `article` 태그를 사용하여 카드 하나를 묶어놓고 공통 클래스명으로 `card-component`를 선언하였습니다.
- `button`은 `ul` 태그를 사용하여 하나의 리스트 그룹으로 묶었습니다.
  - 각각의 `button`에 `aria-label`로 버튼을 누르면 어떤 정보가 뜨는지를 명시해주었습니다. 
- 7개의 카드 전체를 배치할 컨테이너로 `section` 태그를 이용해 클래스명이 `card-container`인 묶음을 선언하였습니다.

```html
<!-- 첫번째 컴포넌트 -->
  <article class="ipad-pro card-component">
    <h2 class="title">iPad Pro</h2>
    <p class="sub-title">놀라우리만치 얇다.<br/>엄청나게 강력하다.</p>
    <p class="callout">출시일 추후 공개</p>
    <ul class="button-group">
      <li><button type="button" aria-label="추가 정보 보기" class="more-btn">더 알아보기</button></li>
      <li><button type="button" aria-label="가격 보기" class="price-btn">가격 보기</button></li>
    </ul>
  </article>
```

<br/>

# :pushpin: CSS

## :pencil2: 컴포넌트 분리

![readme00](/apple/products/01-css-component.png)<br/>

card.css 파일에 폰트사이즈와 컬러, 기본 간격 등을 지정하는 컴포넌트 스타일을 만들고 apple.css에 import로 card.css를 불러와 사용하였습니다.

## :pencil2: card.css

:star: [card.css 바로가기](./styles/card.css)

### 한 개의 카드 내 배치

```css
.card-component {
  height: var(--size);
  display: flex;
  flex-flow: column nowrap;
  align-items: center;
  gap: var(--small-spacing);

  background-size: cover;
  background-repeat: no-repeat;
  background-position: center center;
}
```
- 카드 내부는 flex로 세로 배치하였고 배경이미지는 공통적으로 배치되는 것들을 모아두었습니다.

### 카드 내부 스타일링

```css
.title {
  font-weight: 700;
  font-size: var(--large-text);
  margin-top: var(--large-spacing);
}

.callout {
  font-size: var(--small-text);
  color: var(--gray);
}

.sub-title {
  text-align: center;
  font-size: var(--base-text);
  line-height: var(--line-normal);
}
```
- 먼저 모바일 뷰 기준으로 폰트 사이즈와 색상, 여백 등을 지정하였습니다.

### 버튼 그룹

```css
.button-group {
  display: flex;
  flex-flow: row nowrap;
  gap: var(--base-spacing);
}

.more-btn, .price-btn {
  font-size: var(--xx-small-text);
  padding: var(--x-small-spacing) var(--small-spacing);
  border-radius: 61.25rem;
}

.price-btn {
  background-color: transparent;
}
```
- ul 태그로 묶은 버튼 그룹을 flex 가로 배치하였습니다.
- 각각의 버튼도 스타일링 하였고, '가격 보기' 버튼의 경우에는 배경색을 투명하게 처리하였습니다.

<br/>

### 데스크탑 뷰인 경우 @media 쿼리 사용

```css
/* 데스크탑 */
@media (min-width: 1024px) {
  .title {
    font-size: var(--extra-large-text);
    margin-top: var(--extra-large-spacing);
  }

  br {
    display: inline-block;
    content: " ";
    margin: 0 0.125rem;
  }

  .sub-title {
    font-size: var(--medium-text);
  }

  .more-btn, .price-btn {
    font-size: var(--x-small-text);
  }
}
```
- 데스크탑 뷰로 가로 길이가 1024px이 되면 폰트 사이즈와 여백이 달라지게끔 선언해주었습니다.
- 데스크탑 뷰로 바뀔 때, 부제목에 br 태그로 줄바꿈 처리했던 것을 없애고 좌우 마진값을 주어 공백처럼 처리하였습니다. (구글링해서 얻은 힌트입니다..ㅎ_ㅎ)

<br/>

## :pencil2: apple.css

:star: [apple.css 바로가기](/apple/styles/apple.css)

### import 파일들

```css
@import url(base.css);
@import url(reset.css);
@import url(theme.css);
@import url(card.css);
```
- card.css 파일을 import로 불러오는 것으로 시작하였습니다.

### 전체 컨테이너 레이아웃 지정

```css
/* 레이아웃 */
.card-container {
  display: grid;
  grid-template-columns: 1fr;
  gap: var(--small-spacing);
}
```
- 컨테이너 display는 grid로 주었고 모바일 퍼스트이기 때문에 1fr 1개만 선언하여 1칸짜리 배치를 완료하였습니다.

### 홀수/짝수번째 카드 내부 스타일링

```css
/* 검은 배경 */
.card-component:nth-child(odd) {
  color: var(--white);

  .more-btn, .price-btn {
    border: 1px solid var(--blue-300);
  }

  .more-btn {
    background-color: var(--blue-300);
    color: var(--white);

    &:hover {
      background-color: var(--blue-100);
    }
  }
  .price-btn {
    background-color: transparent;
    color: var(--blue-300);
  }
}
```
- 홀수번째 카드는 어두운 배경이고 짝수번째 카드는 밝은 배경이라 nth-child를 사용하여 카드 내 폰트 색상과 버튼 색상을 맞추었습니다.
- 어두운 배경의 경우, more-btn 의 버튼에 마우스를 호버하면 버튼 색이 좀 더 밝아지는 코드를 넣었습니다.

### 배경 이미지 (image-set)

```css
/* 배경 이미지 */
.ipad-pro {
  background-image: image-set(
    url(/apple/products/ipad_pro.jpeg) 1x,
    url(/apple/products/ipad_pro_2x.jpeg) 2x
  );
}

.ipad-air {
  background-image: image-set(
    url(/apple/products/ipad_air.jpeg) 1x,
    url(/apple/products/ipad_air_2x.jpeg) 2x
  );
}

(중략..)

```
- 배경 이미지는 image-set으로 1배율, 2배율 이미지를 모두 넣어두었습니다.

### 데스크탑 뷰인 경우 @media 쿼리 사용

```css
/* 데스크탑 */
@media (min-width: 1024px) {
  /* 그리드 레이아웃 */
  .card-container {
    grid-template-columns: repeat(2, 1fr);
  }
  .ipad-pro {
    grid-area: 1 / 1 / 2 / 3;
  }
  .ipad-air {
    grid-area: 2 / 1 / 3 / 3;
  }
  .iphone-15-pro {
    grid-area: 3 / 1 / 4 / 3;
  }

  /* 데스크탑용 배경 */
  .ipad-pro {
    background-image: image-set(
      url(/apple/products/ipad_pro_wide.jpeg) 1x,
      url(/apple/products/ipad_pro_wide_2x.jpeg) 2x
    );
  }
  .ipad-air {
    background-image: image-set(
      url(/apple/products/ipad_air_wide.jpeg) 1x,
      url(/apple/products/ipad_air_wide_2x.jpeg) 2x
    );
  }
  .iphone-15-pro {
    background-image: image-set(
      url(/apple/products/iphone15_pro_wide.jpeg) 1x,
      url(/apple/products/iphone15_pro_wide_2x.jpeg) 2x
    );
  }
}
```
- 데스크탑 뷰로 가로 길이가 1024px이 되면 컨테이너가 2개의 열을 갖도록 그리드 템플릿을 바꿔주었고, 맨 앞 3개의 카드는 2개의 열을 모두 차지하여 길게 노출될 수 있도록 grid-area를 지정해주었습니다.
- 데스크탑인 경우 배경이미지가 바뀌는 카드에 각각의 이미지를 1배율, 2배율로 지정해주었습니다.

<br/>

# :pushpin: 구현 결과

![result](/apple/products/03-apple-homework.gif)

<br/>

# :pushpin: 느낀점
- 처음에는 apple.css 파일에 카드 컴포넌트 레이아웃과 스타일링, 전체 컨테이너 레이아웃이랑 스타일 이렇게 전부 다 때려넣으려고 하니 너무 코드가 길어지고 복잡해서, 실습시간에 카드 컴포넌트 파일을 분리해서 작성했던게 생각나 따로 분리하였습니다. 이런게 컴포넌트화라는걸까요? 뭔가 개발자의 길에 한걸음 다가간 느낌이네요.. :relaxed:

- 실제 애플 페이지의 개발자도구를 켜놓고 비교하면서 구현해보았는데 실제로 어떤식으로 개발되고 어떻게 스타일링되는지를 이해할 수 있었습니다. 사실 처음에 버튼 그룹을 div 태그로 묶어놓았는데 실제 애플 홈페이지에서는 ul로 묶어져 있어서 참고하여 저도 ul 태그로 바꾸어 마크업하기도 했습니다. 나중에 버튼이 추가되거나 빠지게 된다면 이렇게 리스트로 관리하는게 유지보수 측면에서 더 좋을 것 같습니다.

- 그리드 레이아웃을 정말 이해하기 어려워서 이것저것 해보다가 겨우 성공했습니다! 이것땜에 하루종일 진땀을 뺐네요. 처음에는 단순히 위에 1~3번 카드까지는 flex로 배치하고 4~7번 카드에만 grid로 배치하자! 라고 생각해서 컨테이너를 나누었었는데요. 이렇게 나누고 보니 스타일링했던게 다 깨지고 배치도 잘 안되서 엄청 고생했습니다. grid 실습시간에 칸을 나누어 연속으로 배치하고 이랬던걸 보면서 전구가 탁! 켜지는 느낌을 받았습니다. 정말.. 어렵지만 해낸 것 같아 감격스러웠습니다. :joy:
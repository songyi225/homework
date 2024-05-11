# :pushpin: 마크업 구조

![markup-static](/images/naver-markup-custom2.png)

<details>
<summary>손그림 펼치기</summary>

![markup](/images/naver-markup-custom.jpeg)

</details>

<br/>

로고 / form (입력 + 버튼) / 하단 로그인 상태 설정 & ip 보안 영역을 묶어서 크게 3군데로 나누었습니다.

### :pencil2: 로고

```html
<h1 class="logo">
  <a href="https://www.naver.com/" target="_blank" class="logo-link">
    <img srcset="/naver/naver-logo.svg, /naver/naver-logo.png" alt="네이버" />
  </a>
</h1>
```
- 로고 이미지는 `<img>`로 마크업하였습니다.
- `srcset` 속성 사용하여 지원하는 브라우저에 따라 이미지가 노출될 수 있도록 svg 와 png 이미지 세트를 넣어 두었습니다.

### :pencil2: 입력 폼 (input + button)

```html
<form action="/" class="login-form">
  <input type="email" id="userEmail" required placeholder="아이디(이메일)" name="userEmail" />
  <input type="password" id="userPwd" required placeholder="비밀번호" name="userPwd" />
  <button type="submit" class="login-btn">로그인</button>
</form>
```

- `<input>` 태그에 `required` 속성을 추가하여 필수값임을 명시하였습니다.

### :pencil2: 로그인 상태 유지와 IP 보안

```html
<div role="group" class="login-setting">
    
  <div class="login-status-keep">
    <input type="checkbox" id="statusKeep" name="statusKeep" />
    <label for="statusKeep">로그인 상태 유지</label>
  </div>

  <div class="ip-security" >
    <a href="/naver/ip_security.html" class="ip-security-info" target="_blank">IP 보안</a>
    <input type="checkbox" id="ipSecurity" name="ipSecurity"/>
    <span class="on-off"></span>
  </div>

</div>
```

- 모바일, 데스크탑 환경에 따라 IP 보안 영역을 보이거나 보이지 않게끔 하기 위해 두 개의 영역을 묶었습니다. (굳이 묶어야할까 고민했지만..)
- IP 보안 영역에서 체크박스 대신 ON/OFF 글자를 가상 요소 선택자로 추가하기 위해 `<span>` 태그를 선언해두었습니다.

<br/>

# :pushpin: CSS 구조

### :pencil2: 공통

```css
body {
  font-size: 1rem;
  color: #181818;
}

.logo {
  width: 230px;
  margin: 100px auto 50px; /* 위아래 여백 임의로 */
}
```
- 공통으로 사용될 전체 글자 크기 16px (1rem), 색상을 먼저 body 안에 넣어두었습니다.
- 로고 가로 너비는 모바일/데스크탑 모두 230px로 고정되어있고, 로고의 여백은 제가 임의로 지정했습니다.

### :pencil2: form 영역

```css
.login-form {
  display: flex;
  flex-flow: column nowrap;
  flex-grow: 1; /* 컨테이너 크기가 커져도 100%를 유지 */
  gap: 0.625rem; /* 임의로 여백 주었음 - 20px */

  input {
    font-size: 0.875rem; /* 14px */
    height: 2.8125rem; /* 45px */
    padding: 0 1.25rem 0 1.25rem;
    /* 플레이스홀더 양쪽 여백 20px */
    border: 1px solid #dadada;
  }
  
  input:focus {
    background-color: #e9f0fd;
    outline: 1px solid #03cf5d;
  }

  /* 버튼 */
  .login-btn {
    font-size: 1rem;
    height: 2.8125rem;
    color: #fff;
    background-color: #03cf5d;
    border: 0;
    margin-top: 1rem; /* 버튼 위쪽 16px 여백 */
  }
}
```
- input과 button 모두 조건에 맞게끔 스타일링하였습니다.
- form 내부에서 중첩하여 각각의 요소에 스타일링을 적용하였습니다.
- input 박스에 focus 시 배경색과 아웃라인 색상 바뀌도록 적용하였습니다.
- button은 별도의 조건이 명시되어 있지 않아서 따로 스타일링하지는 않았지만 button도 포커싱이 됩니다.

### :pencil2: (모바일) 로그인 상태 유지

```css
.login-status-keep {
  position: relative;
  order: 1;

  input {
    position: absolute;
    appearance: none;
    width: 24px;
    height: 24px;
    margin: 0;
  }

  label {
    background: url(/naver/icons/unchecked.svg) no-repeat 0 0 /contain;
    padding-left: 1.5625rem; /* 임의로 25px 여백 */
  }

  input:checked + label {
    background-image: url(/naver/icons/checked.svg);  
  }
}
```

- 기존의 checkbox를 안보이게 `appearance: none;` 처리하고 svg 이미지를 가져와서 label 왼쪽에 배치합니다.
- 기본 스타일링은 모바일 뷰이기 때문에 `order: 1;` 로 순서를 바꾸어 로그인 상태 유지 영역을 오른쪽에 보여주었습니다.
  
### :pencil2: (모바일) IP 보안 영역

```css
.ip-security {
  visibility: hidden;
}
```

- 모바일 뷰에서는 IP 보안 영역을 보이지 않게 처리하였습니다.
- 레이아웃 상 공간을 가지고 있습니다. (이렇게 처리해주는게 베스트일지는 고민이 됩니다.)

### :pencil2: (모바일) 로그인 상태 유지 및 IP 보안 두 영역을 합친 묶음 영역

```css
.login-setting {
  display: flex;
  flex-flow: row nowrap;
  justify-content: space-between;
  padding: 0.625rem 0; /* 위쪽 여백을 위해 10px */
}
```
- 두 컴포넌트를 양쪽에 배치하기 위해 한번 더 묶은 div 태그에 대한 스타일링입니다.

### :pencil2: (데스크탑) 미디어 쿼리 선언

```css
@media (min-width: 768px) {
  ~~
}
```

- 가로크기 768px 이상인 경우 데스크탑으로 간주하여 미디어 쿼리 안쪽의 스타일링을 적용합니다.

### :pencil2: (데스크탑) form 영역 및 하단 묶음 영역

```css
.login-form {
    margin: 0 auto;
    width: 500px;
  }

.login-setting {
    width: 500px;
    margin: 0 auto;
}
```

- form 영역의 가로 크기를 500px로 맞추었습니다.
- 그에 맞춰 하단 묶음 영역도 form과 동일하게 맞추었습니다.
- 좌우 margin을 auto로 처리하여 중앙 정렬시켰습니다.

### :pencil2: (데스크탑) IP 보안 영역

```css
.ip-security {
    visibility: visible;
    order: 1;

    position: relative;

    a {
      text-decoration: none;
    }

    input {
      position: absolute;
      appearance: none;
      width: 24px;
      height: 24px;
    }

    .on-off::after{
      content: "OFF";
      color:#aaa;
      font-size: 100%;
    }

    input:checked + .on-off::after {
      content: "ON";
      color: #03cf5d;
    }
}
```

- 데스크탑 뷰에서는 IP 보안 영역이 보이도록 `visibility: visible;` 처리하였습니다.
- 기존의 checkbox는 보이지 않게 `appearance: none;` 처리하였습니다. 
- span 태그 클래스명인 on-off에 가상 요소 선택자 `::after`를 사용하여 content로 "ON" , "OFF" 텍스트를 추가하였습니다.

<br/>

# 구현 결과

### 반응형 구현 결과

![반응형](/images/반응형-결과.gif)

- 768px 이상인 경우 데스크탑으로 간주
- 768px 미만인 경우 모바일로 간주

<br/>

### 키보드 포커스 구현 결과

![포커스](/images/포커스-결과.gif)

- 키보드 tab키로 포커싱 가능
- 스페이스바로 체크박스 선택 가능

<br/>

### 마우스 클릭 구현 결과

![클릭](/images/클릭-결과.gif)

- 마우스로 선택 가능
- 네이버 로고 선택 시 새 탭으로 네이버 홈 열림
- IP 보안 선택 시 새 탭으로 창 열림

<br/>

# 느낀점 및 아쉬운 점

- 처음 마크업 구조를 그릴 때는 이대로만 만들면 되겠지 싶었는데 실제로 설계를 하다보니 추가하거나 빼야할 것들이 많이 보였습니다. 반복을 통해 구조 설계를 자주 해보아야할 것 같습니다.
- 마크업할 때 접근성을 많이 적용하지 못한 것 같아 아쉽습니다 :sob:
- 반응형 구조를 생각하면서 설계하니 구조가 뒤죽박죽되는 것 같습니다. flex를 이용하여 배치했는데 과제 제출 후에 float를 사용해서 배치하는 것도 연습해볼 예정입니다.
- position의 개념이 잘 잡혀있지 않았는데 이번 과제를 통해 확실히 이해하게된 것 같습니다! :smile:
- 다만, 가상 요소 선택자는 아직도 어렵고 반만 이해한 상태로 과제를 제출하는 것 같습니다. 기존의 요소를 지우고 그 위에 이미지가 아닌 텍스트를 올려놓기 위해 사용하였는데 이 외에 다른 방법이 있을지 좀 더 고민해보려고 합니다. :sob:
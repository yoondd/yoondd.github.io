
## pretendard설치하기

`npm install pretendard`

그리고 `App.js` 또는 `index.js`에

`import 'pretendard/dist/web/static/pretendard.css';`


## _ variables.scss파일 생성

```scss
// styles/_variables.scss

// 폰트 지정
$font-family-base: 'Pretendard', 'Apple SD Gothic Neo', 'Malgun Gothic', '맑은 고딕', sans-serif;

// 사용자 - 색상(Color)
$color-primary: #58419C; /*보라*/
$color-secondary: #8366D8; /*밝은보라*/
$color-point: #FFCA82; /*노랑*/
$color-background: #ffffff; /*흰색*/
$color-graybg: #FAFAFC; /*밝은 회색*/
$color-title: #232323; /*타이틀텍스트-검정*/
$color-text: #6C6C6C; /*일반텍스트-진회색*/
$color-text-secondary: #BEBEBE; /*흐린텍스트-흐린회색*/
$color-warning: #D42222; /*red*/
$color-walk: #ABCD67; /*산책 테마컬러*/


// 관리자 - 색상(color)
$color-primary: #273343; /*진한남색*/
$color-secondary: #505E71; /*밝은남색*/
$color-point: #FFCA82; /*노랑*/
$color-gray: #D9D9D9; /*회색*/
$color-background: #F2F2F2; /*밝은 회색*/
$color-title: #232323; /*타이틀텍스트-검정*/
$color-text: #6C6C6C; /*일반텍스트-진회색*/  
$color-text-secondary: #BEBEBE; /*흐린텍스트-흐린회색*/



// 폰트(Font)
$font-size-xs: 12px;
$font-size-sm: 14px;
$font-size-md: 16px;
$font-size-lg: 20px;
$font-size-xl: 24px;
$font-size-xxl: 32px;
$font-size-xxxl: 48px;

$font-weight-light: 300;
$font-weight-normal: 400;
$font-weight-medium: 500;
$font-weight-bold: 700;
$font-weight-black: 900;

$line-height-sm: 130%;
$line-height-md: 150%;
$line-height-lg: 170%;

$letter-spacing-normal: 0;
$letter-spacing-small: -1px;

// 여백(Spacing)
$space-none: 0;
$space-xxs: 2px;
$space-xs: 4px;
$space-sm: 8px;
$space-md: 16px;
$space-lg: 24px;
$space-xl: 32px;
$space-xxl: 48px;
$space-xxxl: 64px;

// 라운드(Border Radius)
$radius-none: 0;
$radius-sm: 4px;
$radius-md: 8px;
$radius-lg: 16px;
$radius-xl: 32px;
$radius-round: 50%;

// 그림자(Box Shadow)
$shadow-none: none;
$shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
$shadow-md: 0 4px 6px rgba(0,0,0,0.1);
$shadow-lg: 0 10px 20px rgba(0,0,0,0.15);
$shadow-xl: 0 15px 30px rgba(0,0,0,0.2);

// 반응형(Breakpoints)
$breakpoint-mobile: 480px;
$breakpoint-tablet: 768px;
$breakpoint-desktop: 1024px;
$breakpoint-large-desktop: 1200px;


```


## global.scss 에서 위 파일 불러오기

```scss
@import './variables';

body {
  font-family: $font-family-base;
}

```
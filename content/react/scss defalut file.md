총 3개의 기본파일을 만들었다

이 기본파일들은 파일명앞쪽에 언더바(_ )를 붙여서
sass가 자동 번역하지 않도록 오직 불려지는 존재로 만든다

# _ variables.scss
변수를 설정할 때 사용한다.
```scss
$bg-color: #000;
$text-color: #fff;
$accent-color: #ffa500;
```


# _ mixin.scss
믹스인을 사용해서 함수를 설정하고 싶을 때 사용하는데 선생님께서는 반응형을 위해 세팅할 수 있도록 만드셨다. 이렇게 하면 max-width를 한군데서 관리할 수 있어서 매우 편하다는 것을 알 수 있다.
```scss
@mixin respond($viewMode){
	@if $viewMode == pc {
		@media(min-width: 1024px){
			@content;
		}
	}
	@if $viewMode == tablet {
		@media(max-width: 1024px){
			@content;
		}
	}
}
```


# _  base.scss
css reset처럼 사용한다.
```scss 
@use './mixins' as *;
@use './variables' as *;

html {
	font-size: 16px;
	@include respond(tablet){
		font-size: 14px;
	} 
	@include respond(mobile){
		font-size: 12px;
	}
}

* {
	margin: 0; padding: 0;
	box-sizing: border-box;
}
```


# 다른 곳에서는 어떻게 부를까?

App.js에게 내가 만든 디폴트가 무엇인지를 알려주기 위해서 만들어준 파일이 있다.

## main.scss
이 파일은 나의 디폴트들을 모아서 App.js를 알려주는느낌이다
```css
@use './base' as *;
@use './mixins' as *;
@use './variables' as *;
```

별 특별한 기능을 하는 것은 아니고 다른사람들과 함게 협업을 할 때, 디폴트가되는 파일이 어떤것인지 알려주기 위함이고. 보통 이 파일은 그냥 App.js에서만 불러온다


## Section2.scss
이번에는 보통의 컴포넌트에서 어떻게 부르는지 살펴보자.
```css
@use './mixins' as *;
@use './variables' as *;
```

어???? base가 왜없어??? 응. 그건 없어.
걔는 그냥  main에서 부른걸로 충분하더라.

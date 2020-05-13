---
title: "프론트 엔드 개념1"
excerpt: "HTML5, CSS, JavaScript, jQuery"

categories:
 - Front-End
tags:
 - Web
layout: single
---

- HTML : 웹의 구조
- CSS : 웹 페이지를 보기 좋게 꾸며줌
- JavaScript : 웹 페이지의 동작과 이벤트 담당
  - jQuery

# HTML Semantic Elements

<img width="1472" alt="HTML_Semantic_Elements" src="https://user-images.githubusercontent.com/33771279/80434055-aef74700-8933-11ea-8818-082b27810814.png">

# CSS Selector

<style\>

- Grouping Selector : tag1, tag2 { 스타일 지정 }
- ID Selector : #id1 { 스타일 지정 }
- Class Selector : .classname{ 스타일 지정 }
- Universal Selector : *{ 스타일 지정 }
- Descendant Selector : .classname tag1 { 스타일 지정 }
  - 클래스 내부에 있는 태그를 선택한다. 클래스 아래 X.
- Child Selector : .classname > tag1 { 스타일 지정 }
  - 클래스 내부의 자식 레벨만 의미한다.
- Adjacent Sibling Selector : tag1 + tag2 { 스타일 지정 }
  - 두개의 태그가 붙어 있는 경우 적용이 되는데 자신은 적용되지 않는다.
  - 중간에 태그가 끊기면 연속되서 적용되지 않는다.
- General Sibling Selector : tag1 ~ tag2 { 스타일 지정 }
  - 인접 형제, 일반 형제 두 개 다 똑같이 적용된다.
  - 중간에 다른 태그가 있어도 연속되어 적용된다.
- Attribute Selector : div[title] { 스타일 지정 } / div[title="AAA"] { 스타일 지정 }

--------------------------------

- Box Model : header { 스타일 지정 }

</style\>

## CSS Box Model

<img width="910" alt="CSS_Box_Model" src="https://user-images.githubusercontent.com/33771279/80436581-7149ec80-893a-11ea-843d-7aaf3d674980.png">

- **Content** - The content of the box, where text and images appear

- **Padding** - Clears an area around the content. The padding is transparent

- **Border** - A border that goes around the padding and content

- **Margin** - Clears an area outside the border. The margin is transparent

  [출처 : w3school_Box_Model](https://www.w3schools.com/css/css_boxmodel.asp)

# Bootstrap

## Float

```html
<div class="container mt-5">
		<div class="color_box red float-sm-right"></div>
		<div class="color_box yellow float-md-right"></div>
		<div class="color_box darkblue float-lg-right"></div>
		<div class="color_box violet float-xl-right"></div>
	</div>
```

- sm 사이즈가 유지되지 않을 때는 red는 float right가 유지되지 않는다.
- violet : xl 사이즈보다 작아질 때는 float right가 유지되지 않는다.

## Margin & Padding

- m : margin
- mt : margin-top

- pl-sm-5 pl-md-1 pl-lg-5 pl-xl-1 : 반응형 패딩으로 작성하고 싶은 경우
- p-0 ~ p-5 : 반응형 패딩이 아닐 경우
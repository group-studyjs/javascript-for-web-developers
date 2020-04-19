# HTML 속의 자바스크립트

이번 챕터에서는 브라우저가 어떻게 html 문서를 통해 DOM을 해석하며 자바스크립트를 해독하고 실행하는지 간단히 알아보았다. 그리고 \<script\> 태그 내부 요소들에 대해서도 알아보았다.

[1. 브라우저 렌더링,파싱 과정](#1-브라우저-렌더링파싱-과정)

[2. \<script\> 요소](#2-script-요소)

## 1. 브라우저 렌더링,파싱 과정

![](https://d2.naver.com/content/images/2015/06/helloworld-59361-3.png)

브라우저는 렌더링 엔진을 통해 전달받은 HTML 문서를 tree형태(DOM tree)로 구조화하고 CSS와 함께 Rendering tree를 구조화해 화면을 그려준다.

HTML 문서를 읽으면서 DOM tree로 만드는 과정을 파싱이라고 하는데, 파싱은 HTML문서의 \<head\/\> 부터 \<\/body\> 순으로 **위에서 아래로** 읽어내려간다.

그리고 파싱 도중 \<script\> 태그를 만나면 파싱을 중지하고 인라인일경우 해당 \<script\> 를 읽고, 외부 소스일 경우 해당 파일에 접근하고 실행한다. 그래서 화면 렌더링을 빨리 하기 위해서는 \<script\> 태그를 \<\/body\> 바로 위에 위치시키는 것이 좋다.

<hr/>

## 2. \<script\> 요소

| 속성        | 값                                    | 설명                                                 |
| ----------- | ------------------------------------- | ---------------------------------------------------- |
| type        | "text/javscript" `default` / "module" | module 여부                                          |
| src         | "url"                                 | 외부파일의 위치지정                                  |
| async       | booelan                               | 비동기적 fetch, fetch 완료 후, 바로 execution        |
| defer       | boolean                               | 비동기적 fetch, parsing 완료 후, execution           |
| crossorigin | "anonymous" / "use-credentials "      | CORS에 대한 옵션. 대표적으로 쿠키를 주고받을 때 사용 |

### 일반적인 경우

![](https://blog.asamaru.net/res/img/post/2017/05/script-async-defer-1.png)

기본적으로 브라우저가 파싱 중 인라인 스크립트 또는 async, defer, type="module" 특성이 없는 스크립트에 도달하면 스크립트를 가져온 후 실행하기 전까지 분석을 중단한다.

<hr/>

### async 명시한 경우

![](https://blog.asamaru.net/res/img/post/2017/05/script-async-defer-2.png)

**async** 속성이 명시된 경우, 브라우저가 페이지를 파싱되는 동안에도 fetch하고 fetch가 완료되면 파싱도중 스크립트를 실행 한다.

<hr/>

### defer 명시한 경우

![](https://blog.asamaru.net/res/img/post/2017/05/script-async-defer-3.png)

**defer** 속성이 명시된 경우, 브라우저가 페이지의 파싱을 모두 끝냈을 때 스크립트가 실행한다.

<hr/>

## 참고

[http://tcpschool.com/html-tag-attrs/script-async](http://tcpschool.com/html-tag-attrs/script-async)

[https://developer.mozilla.org/ko/docs/Web/HTML/Element/script](https://developer.mozilla.org/ko/docs/Web/HTML/Element/script)

[https://blog.asamaru.net/2017/05/04/script-async-defer/](https://blog.asamaru.net/2017/05/04/script-async-defer/)

[https://monny.tistory.com/185](https://monny.tistory.com/185)

[https://d2.naver.com/helloworld/59361](https://d2.naver.com/helloworld/59361)

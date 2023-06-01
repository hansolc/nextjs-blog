Client-side Navigation

- `Link` 컴포넌트를 통해 Client-side navigation이 가능하다. Client-side navigation이란 페이지 이동이 JavaScript을 통해 이뤄지기 때문에 브라우저가 navigating 하는 것 보다 더 빠르게 동작한다.

Code Splitting and prefetching

- Next.js는 자동적으로 Code Splitting을 하기 때문에 페이지가 많더라도 요청한 페이지가 빠르게 로드된다. 요청한 페이지만 로드 되는 것은 해당 페이지에서 오류가 발생 했더라도 다른 페이지들은 정상적으로 작동한다.
- 또한, `Link` 컴포넌트가 감지될 경우 Next.js는 자동적으로 링크된 페이지들을 background에 미리 가져온다. 따라서 링크를 클릭 할 경우 대상 페이지는 이미 background에 미리 가져왔기 때문에 페이지 전환이 즉각적으로 이루어진다.

Image Component, Image Optimization

- Next.js는 기본적으로 이미지들을 최적화한다. 뷰포트가 더 작은 장치에 큰 이미지가 전달되는 것을 방지하며 빌드 시 이미지를 최적화하는 것이 아닌 사용자가 요청 할 경우 최적화 되어 빌드 시간에는 영향을 주지 않는다. 기본적으로 lazy-loading을 적용한다.

Third-Party JavaScript

- Third-party JavaScript 자원을 가져오기 위해서는 보통 `<head>` 태그에 포함한다. 해당 접근 방식은 작동하지만 언제 로드 될지 명확히 알 수 없고, 특정 스크립트가 페이지 내용을 불러오는데 영향을 끼칠 수 있다. Next.js는 `Sciprt` 컴포넌트를 제공하여 이를 최적화 시키고 성능에 영향이 가지 않도록 도와준다.
-  `strategy`: 언제 스크립트가 불러와야 하는지 설정한다. `lazyOnload`값을 통해 페이지 내용을 불러오는 속도에 지장이 가지 않게 한다.
   `onload`: 스크립트가 불러온 후 실행할 함수를 지정할 수 있다.

CSS Module

- CSS Modules을 통해 CSS 범위를 로컬로 지정하여 고유한 클래스 명을 갖게 한다. 따라서, 같은 이름의 클래스를 다른 파일에 지정해도 클래스 충동에 대한 걱정을 덜어준다.
- CSS Modules을 사용하기 위해서는 `[파일명].[module].[css]` 형태로 파일을 생성하여 css을 작성해 주면 된다.

Global CSS

- Next.js에서 전역으로 지정하고 싶은 CSS는 `/pages/_app.js` 파일에 `globals.css` 파일을 import 하여 사용할 수 있다. Global CSS는 `_app.js` 파일에서만 불러올 수 있다.

Pre-rendering

- Next.js는 기본적으로 모든 페이지에서 `pre-rendering` 작업을 거친다. Next.js는 클라이언트 측에서 모든 JavaScript을 실행하는 대신 미리 각 페이지에서 HTML을 생성한다. 이는 성능적으로, SEO 측면에서도 더 효과적이다.
- Plain React 로 생성된 프로젝트일 경우 JavaScript을 비활성화 할 경우 페이지가 로드되지 않지만 Next.js는 pre-rendering을 통해 브라우저에 HTML이 렌더링 된다. 하지만 hydration 작업을 거치지 못해 링크 작업 같은 컴포넌트간 상호작용은 JavaScript가 실행 되어야 실행 가능하다.

Two forms of Pre-rendering

- 1. Static Generation
      빌드 시 HTML을 생성하여 각 요청마다 이를 재사용 한다.
      주로 Static Generation 방법이 많이 사용되며, 사용자가 요청하기 전에 pre-rendering이 되는 페이지일 경우 더 효율적으로 사용할 수 있다.
- 2. Server-side Rendering
      각 요청마다 HTML을 생성한다.
      페이지에서 데이터가 빈번히 업데이트 되거나 각 요청마다 변경되어야 할 내용이 있을 경우 사용하는 것이 적합하다.
      사용시 `getServerSideProps(context)` 함수를 사용하면 된다.
- Next.js에서는 각 페이지마다 어떤 방식으로 pre-rendering을 할 지 정할 수 있다.

Static Generation

- Static Generation으로 pre-rendering할 경우 외부 데이터가 필요한 경우 빌드 전 데이터 요청이 선행되어야 한다. 이를 `getStaticProps`을 통해 `async`을 걸어주면 된다.

- `getStaticProps`함수는 non-page에서 사용하는 것은 불가능 하다.(React 함수 사용의 경우) React는 페이지가 렌더링 되기 전에 필요한 데이터가 준비 되어 있어야 하기 때문이다.

SWR

- Next.js는 data fetching에 `SWR` React 훅을 사용하길 권장한다. `SWR`은 캐싱, revalidation 등 개발자에게 많은 편의를 제공한다.
-

Dynamic Routes

- `getStaticPaths()`와 `getStaticProps()`함수를 통해 동적 라우팅을 생성할 수 있다. 먼저, 파일명을 생성 할 때 `[]`을 통해 동적 페이지임을 명시한다. 그리고 `getStaticPaths()`를 통해 동적 페이지들을 반환하고 `getStaticProps()`을 통해 해당 데이터를 사용할 컴포넌트에게 전달해 주면 된다.

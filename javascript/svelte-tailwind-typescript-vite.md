# Svelte + Tailwind CSS + TypeScript via Vite

실무에 Svelte를 도입하기 위한 가장 빠른 방법을 찾던 중 발견한 [Vite].
뷰 개발자가 만든 툴이라는 건 알고 있었는데 관심을 두지 않고 있던 사이 꽤 쓸만한 수준까지 도달한 것 같다.  
일단 Native ESM 기반이라 콜드스타트 시에도 번들링이 필요없고, 따라서 HMR이 정말로, 미친 것처럼 빠르다. **안 쓸 이유가 없다**.  

Vite를 이용하여 TypeScript + Svelte + Tailwind CSS 보일러플레이트를 생성한 과정을 기록한다.  
(`svelte@3.49.0`, `vite@3.0.0`, `tailwindcss@3.1.6` 기준)

### Svelte + TypeScript 개발서버 셋업

죄다 원커맨드로 끝낼 수 있다.

```
$ npm init vite
```

프로젝트명을 입력하고, 사용할 framework로 `svelte`를 고른 다음, variant 설정에서 `svelte-ts`를 고르면 끝. 참 쉽죠?  
이후 셋업된 프로젝트 디렉토리로 들어가서 디펜던시 인스톨 후 개발서버를 실행하면 된다.

```
$ cd my-project
$ npm install
$ npm run dev
```

### Tailwind CSS 셋업

1. Tailwind 및 피어 디펜던시를 설치해준다.  
참고로 `svelte-preprocess` 및 `postcss`는 Vite가 자동으로 디펜던시에 넣어주고 활성화까지 해주므로 굳이 따로 설치/설정하지 않아도 된다.

```
$ npm install -D tailwindcss postcss autoprefixer
```

2. 아래 명령어를 통해 `tailwind.config.cjs` 및 `postcss.config.cjs` 설정파일을 생성해준다.

```
$ npx tailwindcss init -p
```

3. `tailwind.config.cjs` 파일을 열어서 아래처럼 `content`를 설정해준다.

```javascript
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ['./src/**/*.{html,js,svelte,ts}'],
  theme: {
    extend: {}
  },
  plugins: []
};
```

4. `src/app.css` 파일 최상단에 아래처럼 directive를 추가해준다.
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

5. **끝**.  
Vite가 제공하는 보일러플레이트 코드 `main.ts`에서 `app.css`를 전역으로 불러와서 넣어주고 있으므로 추가 설정이 필요하지 않다. `app.css`를 사용하지 않으려면 `<style global>` 태그를 이용할 수도 있다.


### 번외편: SCSS 셋업

Tailwind가 커버하지 못하는 영역을 마크업하려면 SCSS를 써야 할 텐데, 당연하게도 Vite에서 빌트인으로 지원한다.  
사용하고자 하는 프리프로세서 디펜던시만 설치해주면 된다.

```
$ npm install -D sass (or node-sass)
```

이후 `<style lang='scss'>`로 스타일 블럭을 선언하면 끝.


[Vite]:https://vitejs.dev/

# WebPack의 Config.js 파일 속성이해하기

## 왜 webpack.config.js 파일을 사용할까 ? 
- npm run build 시 매번 scripts의 build 옵션에 많은 명령어를 길게 추가해야되는 번거로움을 해결해준다.
- 코드예시는 아래와 같다. 
~~~ js
// package.json 파일
{
  "name": "test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build":"webpack --mode=none --entry=src/index/js --output=public/output.js" 
    // 매우 불편함 => webpack.config.js로 대체
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "css-loader": "^6.7.1",
    "mini-css-extract-plugin": "^2.6.1",
    "style-loader": "^3.3.1",
    "webpack": "^5.74.0",
    "webpack-cli": "^4.10.0"
  }
}
~~~

## webpack.config.js 파일에 대해 살펴보고 이에대한 속성에 대해 설명하겠다.
~~~ js
// webpack.config.js
var path = require('path');
var MiniCssExtractPlugin = require("mini-css-extract-plugin");

module.exports = {
  mode: 'none', // production, development, none
  entry: './index.js', 
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ['style-loader', 'css-loader']
      },
    ]
  },
  plugins: [
    new MiniCssExtractPlugin()
  ],
	devtool: 'source-map'
};
~~~




## Mode (웹팩 버전4에서 추가됨)
- 웹팩의 실행 방법을 결정하는 속성이다

### development 모드
- 개발모드이며 번들링시 사이즈가 가장 크다
- 번들링된 파일이 난독화가 되어있지 않고 코드가 압축되어있지않음

### none 모드
- 설정없음모드로 개발모드 다음으로 번들링 사이즈가 가장 큼
- 번들링된 파일이 난독화가 되지않음

### production 모드
- 배포모드이며 번들링 사이즈가 가장 작다
- 번들링된 파일이 난독화가 되어있고 코드가 압축되어있음 



## Source Map
- SourceMap은 개발하는 코드와 번들링된 코드 사이의 관계를 표현하는 데이터로 정의된다.

### 왜 Source Map이 필요할까?
- webpack을 사용해서 번들링을 하게 되면 bundle.js라는 하나의 파일이 만들어지는데 브라우저에서 디버깅할때 난독화된 bundle.js 파일을 디버깅해야됨
  - 난독화된 bundle.js를 디버깅하면 알아보기가 힘드므로 개발하는코드와 번들링된 코드를 연결시키는 sourceMap기능을 사용하면 개발하는 코드를 알아보기 쉽게 디버깅할수있음

## Entry
- 웹팩에서 웹 자원을 변환하기 위해 필요한 최초 진입점이자 자바스크립트 파일경로이다.
 - 빌드,번들링,변환은 같은 의미

### Entry 속성에는 어떤 내용들이 들어갈까?
- entry 속성에 지정된 파일에는 웹 어플리케이션의 전반적인 구조와 내용이 담겨야한다.
  - why ? : 웹팩이 해당 Entry속성 내에 있는 파일을 가지고 웹 어플리케이션에서 사용되는 모듈의 연관관계를 이해하고 분석한다.
    - 즉 웹 어플리케이션을 동작시키는 내용들이 들어가면된다


~~~js
//index.js

import LoginView from './LoginView.js';
import HomeView from './HomeView.js';
import PostView from './PostView.js';

function initApp() {
  LoginView.init(); //로그인화면 
  HomeView.init(); // 메인 화면 
	PostView.init() // 게시글 화면 
}

initApp();

~~~
- 위 코드를 그림으로 나타내면 아래와 같다.
<img width="740" alt="webpack-entry 90e26197 (1)" src="https://user-images.githubusercontent.com/100929485/183249473-18ed30d2-43f6-43d0-a6c7-efe1a37b4b70.png">


### Entry 속성에는 여러가지가 들어갈수있나?
- entry속성안에는 여러가지 js가 들어갈수 있다 
  - 여러개가 들어갈경우 주로 멀티 페이지 애플리케이션에서 활용이된다.  하지만 싱글 페이지 어플리케이션에서는 하나만 들어간다.

## Output
- 웹팩으로 빌드를 하고 난뒤 결과물이 저장된 파일경로를 나타낸다.
- ./dist/bundels.js/에 파일이 생기는 코드 예시
~~~ js
// webpack.config.js
var path = require('path');

module.exports = {
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, './dist')
  }
}
~~~

### Output에 붙이는 이름 옵션 (이거에 대해 좀 더 자세히 알아봐야 할듯)
- 빌드를 할떄마다 고유의 값을 붙여줌
- 코드 예시
~~~js
// 1. 결과 파일 이름에 entry 속성을 포함하는 옵션
module.exports = {
  output: {
    filename: '[name].bundle.js'
  }
};


// 2. 결과 파일 이름에 웹팩 내부적으로 사용하는 모듈 ID를 포함하는 옵션
module.exports = {
  output: {
    filename: '[id].bundle.js'
  }
};

//3. 매 빌드시 마다 고유 해시 값을 붙이는 옵션
module.exports = {
  output: {
    filename: '[name].[hash].bundle.js'
  }
};
// 4. 웹팩의 각 모듈 내용을 기준으로 생생된 해시 값을 붙이는 옵션
// chunkhash같은경우에는 구분자를 줘서 파일이 변화됬다는것을 인식을 시켜 사용자가 강제 새로고침 없이도
// 화면을 잘볼수있게함 
module.exports = {
  output: {
    filename: '[chunkhash].bundle.js'
  }
};
~~~


## Loader (매우 중요하다)
- 웹팩은 웹 애플리케이션을 구성하는 자원(HTML,CSS,Javascript,Images)를 각각의 모듈로보고 이를 조합해서 병합된 하나의 결과물을 만드는 도구이다. 하지만 Javscript뿐만아니라 다른 자원들을 사용하기위해선 Loader가 필요하다.
- webpack.config.js에 module 속성 안에 들어간다.

### Loader가 왜 필요 할까?
- 웹팩은 자바스크립트와 json파일만 해석이 가능하다. 자바스크립트 파일이 아닌 웹 자원 (HTML,CSS,Images,폰트 등)을 사용하려면 로더를 사용하여 js코드 안에 가져와야한다.
  - import 구문으로 해당 파일을 가져옴 (css)


### Loader를 사용해서 CSS를 js 파일로 가져오기 (css-loader,’style-loader’) 사용 예
- 소스코드 예시
  1. webpack.config.js 설정
  2. index.js 파일 에 import
  3. npm run build로 번들링
~~~ js
 //1. webpack.config.js 설정 

var path = require('path');

module.exports = {
  mode: 'none', // production , development ,none
  entry: './index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: { // 로더 부분 
    //규칙 
    rules: [
      {
        test: /\.css$/,
        //로더 종류
        use: ['style-loader', 'css-loader']
      }
    ]
  },
}
// 2. index.js 
// import를 사용해서 css속성을 가져옴 
import './base.css';

// 3. npm run build로 번들링
~~~

### Loader를 사용해서 CSS를 js 파일로 가져오기 (css-loader,’style-loader’) 사용 예 (상세 설명)
- Loader(Module)의 rules의 test 속성은 로더를 적용할 파일 유형을 작성한다 (정규표현식으로 표현)
- Loader(Module)의 rules의 use 속성은 해당 파일에 적용할 로더의 이름이 들어간다 (배열인데 맨끝에서 앞쪽순서로 적용됨 )
- 로더부분(modules)의 use에는 style-loader와 css-loader가 있다 
  - 이 둘의 순서가 바뀌면 정상적으로 번들링이 되지 않는다 [’css-loader,’style-loader’]
    - 왜 정상적으로 번들링 되지 않을까? 
      - style-loader와 css-loader에 대해  알아보자
        - style-loader : 웹팩안에 들어가있는 css 스타일을 head안에 style태그 안에 넣어준다.
        - css-loader: 웹팩안에 css가 들어가게 해준다.
        - 즉 로더는 오른쪽부터 왼쪽으로 반영이된다 즉 1. css-loader , 2.style-loader 인데 두개의 순서가 바뀌게되면 웹팩안에 css가 들어가있지도 않은데 style태그안에 css스타일을 넣으려고하므로 오류가 발생한다

### 자주 사용 하는 Loader 목록 
- Babel Loader
- Sass Loader
- File Loader
- Vue Loader
- TS Loader

### Loader 여러개를 어떻게 넣는예시 
~~~ js
// webpack.config.js
module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' },
      // ...
    ]
  }
~~~

> 각 로더에 대한 설정법과 사용하는방법에 대해 자세히 다루고 있음 https://webpack.js.org/loaders/


## Plugin
- 웹팩으로 변환한 파일(번들돤 파일)에 추가적인 기능을 더하고 싶을떄 사용하는 속성이다.
-  플러그인은 해당 결과물(번들된파일)의 형태를 바꾸는 역할 , 반면 로더는 파일을 해석하고 변환하는 과정에 관여한다.
-  번들링된파일에 추가적인 기능을 더하는것 뿐만아니라 웹팩 변환 과정에 있어 전반에 대한 제어권도 가지고 있음 
![diagram 519da03f](https://user-images.githubusercontent.com/100929485/183278506-2b7ecdff-0c1e-4d93-a185-c0a167c4aacc.png)

-  플러그인에 대한 각 예시
  1. 번들링된 js파일 안에있는 style을 css파일로 추출하는 플러그인 (MiniCssExtractPlugin)
  2. 번들된 CSS,JS파일을 각각의 HTML파일에 태그로 추가해주는 플러그인 (HtmlWebpackPlugin)
  3. 번들링된 결과물에  빌드 정보나 커밋 버전 내용 등을 추가할 수 있음 (BannerPlugin)
  4. 빌드 이전의 결과물을 제거 (CleanWebpackPlugin)

- 많이 쓰이는 플러그인 목록
  - split-chunks-plugin , clean-webpack-plugin, image-webpack-loader,webpack-bundle-analyzer-plugin
  




# 참고자료 
- https://fgh0296.tistory.com/15
- https://javascript.plainenglish.io/why-you-should-not-use-webpack-f07f4fd7c116
- https://www.inflearn.com/course/프런트엔드-웹팩/
- https://tecoble.techcourse.co.kr/post/2021-07-10-webpack-exercise/

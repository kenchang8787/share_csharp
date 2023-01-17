# TFT 前台專案

## 簡述
&nbsp;&nbsp;TFT前台專案目前包括了**遊戲完蛋了**、**Gash數位點**，專案是使用`Vue3+typescript+nginx`架構的獨立前端站台。

主要串接的後端是**TFT-gamefi專案**，專案目前架設於GKE的微服務上。

<br/>

## 專案框架

|名稱|版本|描述|
|-|-|-|
|vue/cli|5.0.4|整個專案的vue3 framework|
|husky|8.0.1|Git hook工具|
|commit-lint|17.0.3|Git commit message工具|
|eslint|x|檢查語法的工具|
|prettier|x|檢查code style的工具|

<br/>

&nbsp;&nbsp;基本上可以在專案的*package.json*中查看所有使用的框架，這邊提一下一些會影響你寫代碼跟操作git的套件。

<br/>

## 專案結構說明

&nbsp;&nbsp;概念上不同專案的某些共用邏輯會完全相同，因為都是串接相同的後端、使用相同的框架跟語言，故抽離出以下共用邏輯的部份，以便快速開發:
- src/apis
- src/services
- src/utils
- src/extensions

<br/>

&nbsp;&nbsp;類別的相依性如下，基本上遵守`外層的不使用內層的類和方法即可`

<br/>

> extensions
>
> > utils
> >
> > > apis
> > >
> > > > services
> > > >
> > > > > components
> > > > >
> > > > > > views

<br/>

&nbsp;&nbsp;所以在開發上若要異動共用邏輯的部分，除了要考慮其他專案是否適用，也需要同步至所有專案。

<br/>

&nbsp;&nbsp;但當在線上的前端專案越來越多的時候，維運所有前端專案會花費不少時間，這會是之後要面對的問題。

<br/>

專案結構以**Gash數位點**為主進行說明:

### project
```
gashex-frontend-web
|--.husky   // git hook設定
|
|--docs     // 所有文件都放在這
|
|--nginx    // nginx伺服器設定
|
|--node_modules
|
|--public
|     favicon.ico   // favicon
|     index.html    // 程式開始處, 並切包括GA、GTM、FBPixel、Beanfun Trace 設定
|
|--src              // 主要的代碼
|
|    .browserslistrc
|    .env.dev           // dev環境變數
|    .env.production    // prudction環境變數
|    .env.stage         // staging環境變數
|    .eslintrc.js       // eslint設定
|    .gitignore 
|    .gitlab-ci.ymal    // 定義了對應環境在commit上gitlab時會執行的任務
|    .prettierignore 
|    .prettierrc.js
|    babel.config.js    // 打包設定, 打包時移除console, comment...等
|    commitlint.config.js   // commitlint設定
|    Dockerfile     // 定義在部署至container時的執行邏輯
|    gashex-frontend-web.code-workspace
|    package-lock.json 
|    package.json   // 定義所有使用的套件和客製化指令
|    README.md
|    tsconfig.json  // typescript設定
|    vue.config.js  // vue設定, webpack打包設定, css,js版本控制
```
<br/>

### src
```
src
|--apis         // gamefi系統所有的API
|  |--base      // API類共用底層
|  |--consumer  // for consumer service
|  |--game      // for game 
|  |--gamefi    // for gamefi
|  |--material  // for material
|  |--oauth     // for oauth
|  |--order     // for order
|  |--payment   // for payment
|
|--assets       // 網站資源(可自行增加, 例如遊戲完蛋了就有下列jsons文件夾)
|  |--css       // copy from docs/UI/css
|  |--images    // copy from docs/UI/images
|  |--jsons     // 依專案使用的json文件
|  |--locales   // 語系資源
|
|--components   // vue元件們
|
|--customs      // 定義依專案使用的方法(例如遊戲完蛋了的質押相關功能)
|
|--extensions   // 定義了typescript既有類別的擴充方法
|  |    array.ts
|  |    date.ts
|  |    index.ts
|  |    number.ts
|  |    string.ts
|  |    window.ts
|
|--router       // 定義路由
|
|--services     // 定義邏輯層
|  |--base      // 定義service類的共用底層
|  |--chain     // 定義鏈相關功能
|  |--invoice   // 定義發票相關功能
|  |--order     // 定義訂單相關功能
|  |--produdct  // 定義商品相關功能
|  |--user      // 定義使用者相關功能
|  |--website   // 定義站台相關功能
|
|--utils        // 定義工具使用方法
|  |--bftrace   // beanfun trace
|  |--common    // 不知道放哪的小方法
|  |--cookies   // cookie使用方法
|  |--crypto    // 加密工具使用方法
|  |--deeplink  // deeplink使用方法
|  |--enum      // 列舉類的擴充方法 (因為列舉沒被ts定義為既有類別, 所以放這)
|  |--environment   // 環境變數使用方法
|  |--g5_oauth  // g5oauth工具使用方法
|  |--i18n  // i18n工具使用方法 (因為每個站台的語系有點不一樣, 所以有考慮放到custom)
|  |--local_storage // local storage使用方法
|  |--route_parameter   // url參數使用方法
|  |--session_storage   // session storage使用方法
|  |--walletconnect // walletconnect使用方法
|
|--views    // 視圖們
|
|    App.vue    // vue視圖的起點，全域工具的初始化、全域css
|    main.ts    // vue框架的起點，有些套件使用須在此引入專案
|    shims-svg.d.ts // 讓ts可以import svg, png file
|    shims-vue.d.ts
```

<br/>

### README

&nbsp;&nbsp;在專案的*README.md*中，我有提供以下資訊:<br/>
- 常用的指令，包括*安裝*、*執行*、*編譯*、*語法檢查*等
- 命名原則，包括*資料夾*、*文件*、*類別*、*方法*、*參數*等名稱
- 資料夾的定義
- 相依性
<br/><br/>

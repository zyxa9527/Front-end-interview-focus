# 20個React的面試重點
## 1. 解釋 React 是什麼
React是一個網頁框架，通過組件化的方式解決視圖層開發復用的問題，本質是一個組件化框架。

它的核心設計思路有三點，分別是聲明式、組件化與通用性。

聲明式的優勢在於直觀與組合。

組件化的優勢在於視圖的拆分與模塊復用，可以更容易做到高內聚低耦合。

通用性在於一次學習，隨處編寫。比如React Native，這裡主要靠虛擬DOM來實現。

這使得React的適用範圍變得足夠廣，無論是Web、Native. VR,甚至Shell應用都可以進行開發。這也
是React的優勢。

但作為一個視圖層的框架，React的劣勢也十分明顯。它並沒有提供完整的解決方案，在開發大型前
端應用時，需要向社區尋找並整合解決方案。雖然一定程度上促進了社區的繁榮，但也為開發者在技術選型
和學習適用上造成了一定的成本。

## 2. 為什麼 React 要使用 JSX

JSX是一個 javascript的語法擴展, 結構類似XML, 像是一種語法糖, 使用 JSX 的方式來提升程式撰寫效率

![未命名](https://user-images.githubusercontent.com/60773919/190983543-e61ed12b-c041-48bc-a77d-b6f4cc5791d8.png)

## 3. React 生命週期?

![未命名](https://user-images.githubusercontent.com/60773919/190988166-2e855bd7-e662-492e-a2f3-1889652a9826.png)

## 4. 類組件跟函數組件有什麼差別

類組件可以繼承, 函數組件更簡單更容易測試, 社群主要推薦函數組件

![未命名](https://user-images.githubusercontent.com/60773919/190990502-28e440dc-caf9-419d-a97f-95545b951d3f.png)

## 5. 如何設計 React 組件

### UI組件 
複用度較高 
- 代理組件:封裝屬性 減少重複code 例:封裝button
- 樣式組件:關注點分離  
```
import classname from 'classnames';
const StyleButton = ({ classNames , primary ...props }) => (
  <Button
    type="button"
    className = {classname('btn',{
        btn-primary : primary    
     },className)}
     {...props} 
  />
);
```
- 布局組件:優化樣式組件 例:主要在於布局組件
```
<Layout
  Top = {<NavigationBat />}
  Content = {<Article />}
  Bottom = {<BottomBar />}
/>
```
### 邏輯組件 
組合UI組件
- 容器組件: 例:fetch , axios
- 高階組件: 參數是組件 返回也是組件 
```
const checkLogin = () => {
  return !!localStorage.getitem('token')
}
const checkLoginFun =  (Acomponent) => {
  return (props)=>{
    return checkLogin() ? <Acomponent {...props} /> : <LoginPage />;
  }
}
class RawUserPage extends React.Component { ... }
const UserPage = checkLoginFun(RawUserPage)
``` 

![未命名](https://user-images.githubusercontent.com/60773919/191149258-fb17cc10-d108-4b53-85b5-e5d0f988db69.png)

### 實踐
使用類似 StoryBook 工具進行管理開發

![image](https://user-images.githubusercontent.com/60773919/191154001-2cb1c9e0-b547-4ef6-a72a-c4072940ae66.png)


## 6.setState 是同步還是異步
在 React 生命週期中是異步的, 使其效能提升, 在原生JS方法中則是同步的
```
class Test extends React.Component {
  state = {
    count : 0
  }
  componentDidMount(){
    this.setState({count:this.setState.count+1});
    console.log(this.setState.count)//0
    
    this.setState({count:this.setState.count+1});
    console.log(this.setState.count)//0
    
    setTimeout(()=>{
      this.setState({count:this.setState.count+1});
      console.log(this.setState.count)//2

      this.setState({count:this.setState.count+1});
      console.log(this.setState.count)//3
    },0)
  }
}
```

![未命名](https://user-images.githubusercontent.com/60773919/191149946-cb0bc623-1f2d-4fd2-8323-f48cde061292.png)

## 7.跨組件溝通

![image](https://user-images.githubusercontent.com/60773919/191157316-730f179d-6fbf-4a6b-9830-4e0fa974f6d0.png)


## 8.React 狀態管理框架

單一數據流 , state是只讀的

### Redux的核心组成有三部分
- Store：存儲數據狀態的容器
- Action：動作
- Reducer：一個函數，接受兩個參數，第一個參數是當前的state，第二個參數是action。 reducer負責根據action對狀態進行處理。為什麼叫Reducer，是因為Reducer函數可以作為Array的reduce函數的參數

## 9.Virtual DOM 的原理是什麼

Virtual DOM 的運作原理是通過 js 對象模擬 DOM 節點, 在 facebook 初期構建 React 時, 因考慮到提升code抽象能力, 避免人為的 DOM 操作, 降低code整體風險的因素, 所以引入了 Virtual DOM
在更新真實的 DOM 之前，會先透過 diff 演算法比較新的 Virtual DOM 與舊的 Virtual DOM，然後再改變 DOM 的節點

### 優點
- 改善大規模操作效能
- 避免XSS攻擊
- 低成本跨平台開發 

### 缺點
- 內存占用較高
- 難以優化

![image](https://user-images.githubusercontent.com/60773919/191192994-dc262639-5a66-4843-8e6b-ca711f8f53a1.png)

## 10.與其他框架相比, diff 演算法有何不同

React 的 diff 算法, 觸發更新的時機主要在 state 的變化或是 props 調用之後, 此時觸發 Virtual DOM 樹遍歷, 採用深度演算法, 但傳統的遍歷方式效率較低, 為了優化效率, 採用了分治的方式, 將單一節點比對轉換成三種類型節點比對, 分別是樹、 組件、 元素, 以此來提升效率

- 樹比對, 由於網頁中較少有跨層級的節點移動, 兩株 Virtual DOM 樹只對同一層級節點進行比較
- 組件比對, 如果組件是同一類型, 則進行樹比對, 如果不是, 則放入更新中
- 元素比對, 主要發生在同層級中, 通過標記節點, 生成更新

### 如何優化
- 根據 diff 算法的設計原則, 應盡量避免跨層級節點移動
- 通過設置唯一 key 進行優化, 盡量減少組件層級深度
- 因為過深的層級會加深遍歷深度, 帶來效能問題, 設置 shouldComponentUpdata 或 React.pureComponent 減少 diff 次數

![image](https://user-images.githubusercontent.com/60773919/191195960-d25536ea-2531-4ca2-88cd-a27a2db06adb.png)

## 11.如何解釋 React 的渲染過程(進階)

- [參考文章1](https://segmentfault.com/a/1190000041112179)
- [參考文章2](https://juejin.cn/post/6959120891624030238)
- [參考文章3](https://www.51cto.com/article/685903.html)

![image](https://user-images.githubusercontent.com/60773919/191203996-ac159504-7c90-4713-ac22-397479939afb.png)

## 12.React 的渲染異常會造成什麼後果
React 渲染異常的時候, 在沒有做任何攔截的情況下, 會出現頁面白屏的現象, 他的呈現原因是在渲染層出現 javascript 錯誤, 導致整個應用崩潰, 這種錯誤通常是在 render 中沒有控制好空安全, 取到了空值

![image](https://user-images.githubusercontent.com/60773919/191206911-e9518b7f-0c9c-4064-a204-de46485e1aeb.png)

## 13.如何分析和優化效能

### 指標
- FCP 首次繪製內容的耗時
- TTI 頁面可互動的時間
- Page Load 頁面加載時間
- FPS 前端頁面偵率
- 靜態資源及 API 成功率

### 效能
通常效能優化必須配合公司業務的情況, 來找尋適合的優化指標, 這裡我舉之前負責過的專案, 在進行優化前, 用戶一開始開啟網頁時需耗時5秒多的時間, 根據 GOOGLE 的研究顯示當用戶開啟網頁時間超過3秒則會降低用戶的留存率, 所以我根據他們的 RAIL 模型指標, 使用 Lighthouse 工具進行檢測, 查看哪些分數是較低的, 首先我使用預渲染的方式將SPA轉成類似SSR的模式來提升首次繪製內容的耗時, 再來使用懶加載的方式還有Webpack打包共同的chunk讓頁面加載時間變快, 對於靜態資源則盡量使用CDN, 域名解析失敗就自動切換方案, 在經過優化之後, 開啟網頁的時間下降到了2秒多, 速度上升了75%

### SEO
而 SEO 的部分, 首先我使用 JSON-LD 處理結構化資料, 並且生成 sitemap 的 xml 文件, 提交到 google 上形成網站地圖, 另外我還使用了PWA增進用戶的體驗

![image](https://user-images.githubusercontent.com/60773919/191420418-9029866b-43cc-4417-b259-1248bef10712.png)

## 14.如何避免重複選染
首先要先評估是否需要進行優化, 例如不同的瀏覽器或是過慢的網速是否有必要優化, 確定情況之後, 可以先使用 Performance 工具進行檢測, 看是哪個地方造成卡頓的問題, 通常是使用 usememo 的方式來緩存資料, 也可以使用 immer.js不可變數據來減少重複渲染

![image](https://user-images.githubusercontent.com/60773919/191421370-929c888d-8711-4dc9-b6cc-7cb47b767ed2.png)

## 15.如何提升 React 程式碼可維護性
### 可分析
- Code Review
- ESLint
### 可擴展
- 組件設計模式
- 狀態框架Redux
### 穩定性
- 單元測試
### 可測試
- 純函數

![image](https://user-images.githubusercontent.com/60773919/191423522-05124463-b1d9-4814-a024-234500c8d7c9.png)

## 16.React Hook 的使用限制有哪些
不要在循環或條件下使用 Hook, 要在 React 函數組件中調用, 可以使用 ESLint 避免

為何要使用 React Hook
- 組件難以複用邏輯
- 複雜組件難以拆分
- this容易混淆

![image](https://user-images.githubusercontent.com/60773919/191428245-c862cf73-8ced-44c0-9a1a-11ae3f27834a.png)

## 17.useeffect 和 useLayouteffect 差別在哪
如果有直接操作 DOM 或是畫面閃爍則推薦使用 useLayouteffect, 其他功能一樣, 會在頁面渲染完成時調用, 並且提供第二個參數去 dependencies 是否改變接著調用

![image](https://user-images.githubusercontent.com/60773919/191432634-669ba450-843e-45e2-b273-74c4c8c9073a.png)

## 18.React Hook 的設計模式
拋棄生命週期的思考方式, 採用外觀模式, 把邏輯封裝到各自的自訂 Hook 中
例: 過去類組件的開發模式中, 在mount放監聽事件後, 還需要取消監聽, 在 Hook 思路上可以把監聽與取消監聽放到同一個 useeffect 中, useeffect不需要關心生命週期, 只需要關心外部依賴

## 19.React Router 的設計原理和工作方式

![image](https://user-images.githubusercontent.com/60773919/191449235-2869a6c9-0c8e-4015-a378-48c1b7d2f1ee.png) 

## 20.React 中常用的工具
- 初始化 create-react-app
- 路由 React-router
- 樣式 styled-components
- 基礎組件 Antd
- 功能組件 
  1. react-dnd, react-draggable 拖曳
  2. react-pdf-viewer 預覽PDF
  3. Video-React 播放影片
  4. react-window, virtualized 解決長列表
- 狀態管理 Redux
- 打包 webpack, esbuild
- 規範 ESLint
- 測試 jest, react-testing-library
- 發布 s3-plugin-webpack插件處裡靜態資源上傳

![image](https://user-images.githubusercontent.com/60773919/191454479-be2e7b13-6fd0-496b-8f55-2da031f2b5db.png) 

## 參考影片
[前端知識](https://www.youtube.com/watch?v=Ku4ejcTpteo)
[演算法](https://www.youtube.com/watch?v=d-Lj4s6307c)

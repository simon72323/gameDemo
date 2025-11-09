## 整合引入代碼

建議把不必要的css style與標籤移除，尤其是bootStrape.css、swiper-bundle.min.js、Promotion_css.css、in_game_menu_css，提及的這幾個是務必要移除並添加以下

```html
<head>
  <script>
    fetch("/gamePlugin/index.html")
      .then((response) => response.text())
      .then((htmlString) => {
        // 创建一个临时的 DOM 元素来解析 HTML 字符串
        const tempDiv = document.createElement("div");
        tempDiv.innerHTML = htmlString;

        // 提取 script 元素并添加到 head 中
        const scriptElements = tempDiv.querySelectorAll("script");
        scriptElements.forEach((script) => {
          const newScript = document.createElement("script");
          // 复制所有属性
          Array.from(script.attributes).forEach((attr) => {
            newScript.setAttribute(attr.name, attr.value);
          });
          document.head.appendChild(newScript);
        });

        // 提取 link 元素并添加到 head 中
        const linkElements = tempDiv.querySelectorAll('link[rel="stylesheet"]');
        linkElements.forEach((link) => {
          const newLink = document.createElement("link");
          // 复制所有属性
          Array.from(link.attributes).forEach((attr) => {
            newLink.setAttribute(attr.name, attr.value);
          });
          document.head.appendChild(newLink);
        });
      })
      .catch((error) => {
        console.error("Error fetching the HTML:", error);
      });
  </script>
</head>
<body>
  <!-- 建議在遊戲標籤後面 -->
  <div id="react-root"></div>
</body>
```

## 串接命令

```ts
// 遊戲初始化完畢後開啟相關功能（請務必在遊戲初始完，loading100%完成時調用）
__GAME_PLUGIN__.dispatch({
  type: "setting/setFlags",
  payload: {
    // 遊戲列表
    gameListDialogEnable: true,
    // Promotion計分板
    promotionTableDialogEnable: true,
    // Promotion提示
    promotionDialogEnable: true,
    // Cash Drop Reward Popup提示
    cashDropRewardPopupEnable: true,
    // 遊戲說明框
    gameInfoDialogEnable: true,
  },
});

// 打開Promotion計分板
__GAME_PLUGIN__.dispatch({
  type: "setting/setPromotionTableDialogShow",
  payload: true,
});

// 打開遊戲列表視窗
__GAME_PLUGIN__.dispatch({
  type: "setting/setGameListDialogShow",
  payload: true,
});

// 打開遊戲說明
__GAME_PLUGIN__.dispatch({
  type: "setting/setGameInfoDialogShow",
  payload: true,
});
```

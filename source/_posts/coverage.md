title: 使用chrome 精简多余的css和js
date: 2018-11-21 10:52:55
tags: node
---

在chrome的控制台选项中，有一项名为“Coverage”的功能，使用它可以统计页面中js和css的使用率。

但是，它暂不支持直接导出所有使用到的资源。

对于一个已经结束功能开发的页面，还是可以做一下精简的。

以下是使用 Puppeteer 调用chrome的api提取利用到的css的代码

```javascript
// 导出页面中使用到的css
let puppeteer = require('puppeteer');
(async () => {
  const browser = await puppeteer.launch();
  const page = await browser.newPage()

  //Start sending raw DevTools Protocol commands are sent using `client.send()`
  //First off enable the necessary "Domains" for the DevTools commands we care about
  const client = await page.target().createCDPSession()
  await client.send('Page.enable')
  await client.send('DOM.enable')
  await client.send('CSS.enable')
  console.log('client ready')

  const inlineStylesheetIndex = new Set();
  client.on('CSS.styleSheetAdded', stylesheet => {
    console.log('CSS.styleSheetAdded')
    const { header } = stylesheet
    if (header.isInline || header.sourceURL === '' || header.sourceURL.startsWith('blob:')) {
      inlineStylesheetIndex.add(header.styleSheetId);
    }
  });

  //Start tracking CSS coverage
  const tracking = await client.send('CSS.startRuleUsageTracking')
  console.log('startRuleUsageTracking', tracking)

  let pageRes = await page.goto(`https://www.shoppingmg.com/Items/Details/6/0`)
  console.log('page res back')
  const content = await page.content();
  console.log('page content loaded');

  const rules = await client.send('CSS.takeCoverageDelta')
  const usedRules = rules.coverage.filter(rule => {
    return rule.used
  })

  const slices = [];
  for (const usedRule of usedRules) {
    // console.log(usedRule.styleSheetId)
    if (inlineStylesheetIndex.has(usedRule.styleSheetId)) {
      continue;
    }

    const stylesheet = await client.send('CSS.getStyleSheetText', {
      styleSheetId: usedRule.styleSheetId
    });

    slices.push(stylesheet.text.slice(usedRule.startOffset, usedRule.endOffset));
  }

  const fs = require('fs');

  fs.writeFile('app.min.css', slices.join(''), (err) => {
    if (err) throw err;
    console.log('The css file has been saved!');
  });

  await page.close();
  await browser.close();
})();
```

亲测发现fonts资源需要手动补充，原来118k的css砍完剩下7k

一次导出js和css 👉 https://github.com/istanbuljs/puppeteer-to-istanbul
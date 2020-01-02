# 浏览器渲染过程

1. DOM Tree： html
2. Css Rule Tree： css
3. Render Tree: Dom 和Css合并生成，不知道节点位置，需要layout
4. layout：计算节点在屏幕中的位置
5. painting： 按照算出来的规则，通过显卡，把内容画到屏幕上。
6. display： 打击看到的最终效果。

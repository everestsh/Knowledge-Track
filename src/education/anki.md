# Anki 记忆卡片

让知识要点更容易，更牢靠。

## 背景理论

- [间隔重复](https://zh.wikipedia.org/wiki/%E9%97%B4%E9%9A%94%E9%87%8D%E5%A4%8D)

利用心理学[间隔效应](https://zh.wikipedia.org/w/index.php?title=间隔效应&action=edit&redlink=1)，通过不断复习所学内容并逐步增加两次复习间的时间间隔来提升效率的学习技巧。

## 核心概念

- **Cards 卡片**： 一对「问题」与「答案」，对应卡片正反面🎴

- **Decks 卡堆**：由一组卡片组成，可包含其他卡堆。`::` 用于指定卡堆层级。

- **Notes & Fields 笔记**：自定义链接有对应关系的卡片，以便结合记忆。

- **Card Type 卡片类型**：定义卡片类模板。

```
Q: Bonjour
A: Hello
   Page #12
-----
Q: {{French}}
A: {{English}}<br>
   Page #{{Page}}
```

- **Note Types 笔记类型**：

  - Basic
  - Basic (and reversed card)
  - Basic (optional reversed card)
  - Cloze

- **Collection 集合**：储存所有 Anki 资源

## 共享卡堆

- [获取](https://ankiweb.net/shared/decks/)共享卡堆

## 链接

- [MDAnki](https://github.com/ashlinchak/mdanki) - 通过 Markdown 生成 Anki 卡片

--------------------------------------------------------------------------------

附：[完整帮助文档](https://docs.ankiweb.net/#/studying)

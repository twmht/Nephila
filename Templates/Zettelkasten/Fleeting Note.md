---
title: <% tp.date.now("YYYYMMDDHHmmss") %>-<% tp.file.cursor(0) %>
date: <% tp.date.now("YYYY-MM-DD HH:mm") %>
tags:
---

## 想法 / Observation
- <% tp.cursor(1) %>

## 提問 / Question
- <% tp.cursor(2) %>

## 後續行動 / Follow-up
- [ ] 完成相關閱讀: <% tp.file.title %>
- [ ] 撰寫一篇相關永久筆記
- [ ] <% tp.cursor(3) %>

## 相關內容 / Context
- 當前筆記: <% tp.file.title %>
- 關聯標籤: <% tp.frontmatter.tags %>
- 引用筆記:
  - <% tp.file.find_tfile("example_reference") %>




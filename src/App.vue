<template>
  <!-- <img alt="Vue logo" src="./assets/logo.png" /> -->
  <!-- <HelloWorld msg="Hello Vue 3 in CodeSandbox!" /> -->
  <ol>
    <template v-for="item in toc" :key="item.id">
      <div v-if="item.tag === 1">
        <li>
          <a :href="`#toc-nav${item.id}`" v-html="item.text"></a>
        </li>
      </div>
      <div v-if="item.tag === 2">
        <ul>
          <li><a :href="`#toc-nav${item.id}`" v-html="item.text"></a></li>
        </ul>
      </div>
      <div v-if="item.tag === 3" class="tag-6">
        <span></span>
        <a :href="`#toc-nav${item.id}`" v-html="item.text"></a>
      </div>
      <div v-if="item.tag === 4">
        <li>
          <a :href="`#toc-nav${item.id}`" v-html="item.text"></a>
        </li>
      </div>
      <div v-if="item.tag === 5">
        <ul>
          <li><a :href="`#toc-nav${item.id}`" v-html="item.text"></a></li>
        </ul>
      </div>
      <div v-if="item.tag === 6" class="tag-6">
        <span></span>
        <a :href="`#toc-nav${item.id}`" v-html="item.text"></a>
      </div>
    </template>
  </ol>
  <div id="content"></div>
</template>

<script>
import { marked } from "marked";
import markdownText from "./assets/test.md?law";
import hljs from "highlight.js";
import "highlight.js/styles/foundation.css";
export default {
  name: "App",
  data() {
    return {
      toc: [],
    };
  },
  mounted() {
    const render = new marked.Renderer();
    marked.setOptions({
      renderer: render, // 这是必填项
      gfm: true, // 启动类似于Github样式的Markdown语法
      pedantic: false, // 只解析符合Markdwon定义的，不修正Markdown的错误
      sanitize: false, // 原始输出，忽略HTML标签（关闭后，可直接渲染HTML标签）
      // 高亮的语法规范
      highlight: (code) => hljs.highlightAuto(code).value,
      heading: (text, level) => text + level,
    });
    let anchor = 0;
    render.heading = (text, level, raw) => {
      console.log(text);
      //保存这篇文章的最大标签
      if (level < this.maxTitle) {
        this.maxTitle = level;
      }
      anchor += 1;
      /* 
     toc:数组用于保存标题，
     id:标题id,用于点击目录滚动到改标题
     tag:记录属于那个标签（h1……h6）
     test:标签内容
     */
      this.toc.push({
        id: anchor,
        tag: level,
        text: text,
      });
      return `<h${level} id="toc-nav${anchor}">${text}</h${level}>`;
    };
    console.log(this.toc);

    document.getElementById("content").innerHTML = marked(markdownText);
  },
};
</script>

<style>
.tag-6 {
  display: flex;
  align-items: center;
}
.tag-6 span {
  display: inline-block;
  width: 7px;
  height: 7px;
  background: black;
  margin: 0 10px 0 35px;
}
</style>

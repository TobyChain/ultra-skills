# 一个超简版示例（结构骨架）

> 这是参考用的极简骨架，**真实输出**要按 SKILL.md 中的 12 章节规范展开，每章字数 / 内容深度都要远超这里。

```html
<section id="sec-1">
  <h2><span class="num">01</span>一句话理解</h2>
  <p>Genkit 是 Google Firebase 出品的统一 AI 应用框架……</p>
</section>

<section id="sec-2">
  <h2><span class="num">02</span>项目卡片</h2>
  <table>
    <tr><th>名称</th><td>Genkit</td></tr>
    <tr><th>语言</th><td>TypeScript / Go / Python (beta) / Dart (preview)</td></tr>
    <tr><th>License</th><td>Apache-2.0</td></tr>
    <tr><th>仓库</th><td><a href="https://github.com/genkit-ai/genkit">genkit-ai/genkit</a></td></tr>
  </table>
</section>

<section id="sec-3">
  <h2><span class="num">03</span>为什么存在</h2>
  <p>每家模型厂商有自己的 SDK，应用层要在 OpenAI / Gemini / Claude / Ollama 之间切换时……</p>
  <blockquote>核心观察：把"调模型"做成像"调数据库"一样的标准化能力。</blockquote>
</section>

<section id="sec-4">
  <h2><span class="num">04</span>架构鸟瞰</h2>
  <div class="mermaid">
flowchart LR
  App[业务代码] --> AI[Genkit 实例]
  AI --> Plugins[Plugin 系统]
  AI --> Flow[Flow 引擎]
  Plugins --> GoogleAI
  Plugins --> OpenAI
  Plugins --> Anthropic
  Flow --> Trace[追踪/观测]
  </div>
</section>

<section id="sec-6">
  <h2><span class="num">06</span>关键代码 + 大白话</h2>
  <div class="code-pair">
    <pre><code class="language-ts">
const ai = genkit({ plugins: [googleAI()] });
const { text } = await ai.generate({
  model: googleAI.model('gemini-2.5-flash'),
  prompt: '解释一下重力',
});
    </code></pre>
    <div class="explain">
      <p>第 1 行：创建一个 Genkit 实例，把 Google AI 这块"插件"插进去。</p>
      <p>第 2-5 行：调用 generate 来一次模型推理，不管底下是 Gemini 还是 Claude，长得都一样。</p>
      <p>关键点：插件让"换模型"变成"换字符串"，而不是改一堆代码。</p>
    </div>
  </div>
</section>

<section id="sec-9">
  <h2><span class="num">09</span>常见误区</h2>
  <div class="pitfall">
    <span class="label">误区</span>
    <p>以为 Flow 就是普通函数</p>
    <p class="truth"><strong>真相：</strong>Flow 是被追踪、可重放、可观测的"带元数据的函数"，所以才需要 defineFlow 包一层。</p>
  </div>
</section>

<section id="sec-11">
  <h2><span class="num">11</span>自测题</h2>
  <details class="quiz">
    <summary>Q1. 如果把 plugins: [googleAI()] 改成 plugins: []，generate() 调用会发生什么？</summary>
    <div class="answer">
      会在运行时抛错，提示 model not registered。因为模型是通过 plugin 注册到 ai 实例的，没有 plugin 就没有可用模型。
    </div>
  </details>
</section>
```

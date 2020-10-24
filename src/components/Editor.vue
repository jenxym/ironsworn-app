<template>
  <div class="editor-box">
    <div v-if="editorMenuShow" class="editor-menu">
      <editor-menu-bar :editor="editor" v-slot="{ commands, isActive }">
        <div class="menu-buttons">
          <button :class="{ 'is-active': isActive.heading({ level: 1 }) }" @click="commands.heading({ level: 1 })">H1</button>
          <button :class="{ 'is-active': isActive.heading({ level: 2 }) }" @click="commands.heading({ level: 2 })">H2</button>
          <button :class="{ 'is-active': isActive.heading({ level: 3 }) }" @click="commands.heading({ level: 3 })">H3</button>
          <button :class="{ 'is-active': isActive.bold() }" @click="commands.bold">B</button>
          <button :class="{ 'is-active': isActive.italic() }" @click="commands.italic">I</button>
          <button :class="{ 'is-active': isActive.code_block() }" @click="commands.code_block">Code</button>
          <button :class="{ 'is-active': isActive.undo }" @click="commands.undo">Undo</button>
          <button :class="{ 'is-active': isActive.redo }" @click="commands.redo">Redo</button>
        </div>
      </editor-menu-bar>
    </div>
    <div class="editor-content">
      <editor-content :editor="editor" />
    </div>
  </div>
</template>

<script>
import { Editor, EditorContent, EditorMenuBar } from "tiptap";
import { Heading, Bold, Italic, History, TrailingNode, CodeBlock } from "tiptap-extensions";
export default {
  components: {
    EditorContent,
    EditorMenuBar,
  },
  props: {
    editorMenuShow: {
      type: Boolean,
      default: true,
    },
    textDefault: {
      type: String,
      default: `
      <h1>Título</h1>
      <p>Introduce texto...</p>`,
    },
    advancedEditor: {
      type: Boolean,
      default: true,
    },
  },
  data() {
    return {
      editor: new Editor({
        extensions: [
          new Bold(),
          new Italic(),
          new History(),
          new Heading({ levels: [1, 2, 3] }),
          new TrailingNode({
            node: "paragraph",
            notAfter: ["paragraph"],
          }),
          new CodeBlock(),
        ],
        content: this.textDefault,
        // `
        // <h1>Yo soy un título</h1>
        // <p>Soy un párrafo :D</p>
        // <pre><code>Soy u códugo</code></pre>
        // `,
      }),
    };
  },
  // mounted() {
  //   console.log(this.editor);
  //   console.log(this.editor.view.dom);
  // },
  beforeDestroy() {
    this.editor.destroy();
  },
};
</script>

<style>
.editor-box {
  margin: 20px 0;
  border: 2px solid #b4b4b4;
  border-radius: 5px;
}
.editor-menu {
  background: #b4b4b4;
  padding: 10px;
}
.menu-buttons button {
  background: #eee;
  padding: 5px;
  margin: 2px;
  border: 1px solid #ccc;
  cursor: pointer;
  color: black;
  font-size: 16px;
  font-weight: bold;
  border-radius: 4px;
  min-width: 32px;
}

.menu-buttons .is-active {
  background: #9c9c9c;
}

.editor-content pre {
  margin: 10px 10px;
  padding: 0.7rem 1rem;
  border-radius: 5px;
  background: #000;
  color: #fff;
  font-size: 16px;
  overflow-x: auto;
}
</style>

<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png" />
    <HelloWorld msg="Welcome to Your Vue.js App" />
    <div>
      <Editor :editor-menu-show="false" :text-default="`Soy una caja`" />
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from "@/components/HelloWorld.vue";
import Editor from "@/components/Editor.vue";

export default {
  name: "Home",
  components: {
    HelloWorld,
    Editor,
  },
};
</script>

<template>
  <div class="d-block w-100p m-xl-2x bd-1 bd-rds-xs">
    <menu-bar :editor="editor" /> 
    <editor-content class="d-block w-100p bdt-1 p-1x" :editor="editor" />
  </div>
</template>

<script>
// Editor
import { Editor, EditorContent  } from "tiptap";
import extensions from '../extensions/index.js'
import { content } from '../constants/content.js'
// Components
import MenuBar from './MenuBar.vue'

export default {
  name: "Editor",
  components: {
    EditorContent,
    MenuBar,
  },
  data() {
    return {
      previewSource: [],
      editor: new Editor({
        extensions,
        content
      }),
    };
  },
  beforeDestroy() {
    this.editor.destroy();
  },
  methods: {
    onFileSelect(file) {
      this.previewSource.push(file)
    },
    apply(command) {
      this.previewSource.forEach(file => {
        command({src: file.src})
      })
      this.previewSource = [];
    }
  }
};
</script>
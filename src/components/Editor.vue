<template>
  <div>
    <div class="d-block w-100p m-xl-2x bd-1 bd-rds-xs">
      <menu-bar :editor="editor" />
      <button @click="switchMode">Switch</button>
      <editor-content
        v-if="normalMode"
        class="d-block w-100p bdt-1 p-1x"
        :editor="editor"
      />
      <div class="htmlMode" v-else>
        <textarea v-model="html" class="d-block w-100p p-1x"></textarea>
        <div class="actions">
          <button class="button" @click="clearContent">Clear Content</button>
          <button class="button" @click="setContent">Set Content</button>
        </div>
      </div>
    </div>
    <div>{{ html }}</div>
  </div>
</template>

<script>
// Editor
import { Editor, EditorContent } from 'tiptap'
import extensions from '../extensions/index.js'
import { content, html } from '../constants/index.js'
// Components
import MenuBar from './MenuBar.vue'

export default {
  name: 'Editor',
  components: {
    EditorContent,
    MenuBar,
  },
  data() {
    return {
      previewSource: [],
      editor: new Editor({
        extensions,
        content,
        onInit: ({ state }) => {
          console.log(state)
        },
        onUpdate: ({ state, getHTML, getJSON, transaction }) => {
          this.html = getHTML()
          console.log(getJSON())
        },
      }),
      html,
      normalMode: true,
    }
  },
  beforeDestroy() {
    this.editor.destroy()
  },
  methods: {
    onFileSelect(file) {
      this.previewSource.push(file)
    },
    apply(command) {
      this.previewSource.forEach((file) => {
        command({ src: file.src })
      })
      this.previewSource = []
    },
  },
  methods: {
    clearContent() {
      this.editor.clearContent(true)
      this.editor.focus()
    },
    setContent() {
      console.log(this.html)
      this.editor.setContent(this.html)
    },
    switchMode() {
      this.normalMode = !this.normalMode
    },
  },
}
</script>

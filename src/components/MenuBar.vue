<template>
  <editor-menu-bar :editor="editor" v-slot="{ commands }">
    <div class="menubar">
      <button
        class="menubar__button btn btn-icon medium"
        @click="commands.bold"
      >
        <i class="icon icon-te-bold icsz-16"></i>
      </button>

      <button
        class="menubar__button btn btn-icon medium"
        @click="commands.italic"
      >
        <i class="icon icon-te-italic icsz-16"></i>
      </button>

      <button
        class="menubar__button btn btn-icon medium"
        @click="commands.strike"
      >
        <i class="icon icon-te-strike icsz-16"></i>
      </button>

      <button
        class="menubar__button btn btn-icon medium"
        @click="commands.underline"
      >
        <i class="icon icon-te-underline icsz-16"></i>
      </button>

      <button
        class="menubar__button btn btn-icon medium"
        @click="commands.code"
      >
        <i class="icon icon-te-code icsz-16"></i>
      </button>

      <button
        class="menubar__button btn btn-icon medium bg-inherit"
        @click="commands.heading({ level: 1 })"
      >
        <span>H1</span>
      </button>

      <button
        class="menubar__button btn btn-icon medium"
        @click="commands.heading({ level: 2 })"
      >
        H2
      </button>

      <button
        class="menubar__button btn btn-icon medium"
        @click="commands.heading({ level: 3 })"
      >
        H3
      </button>

      <button
        class="menubar__button btn btn-icon medium"
        @click="commands.bullet_list"
      >
        <i class="icon icon-task-list icsz-16"></i>
      </button>
      <button
        class="menubar__button btn btn-icon medium"
        @click="commands.aligntext({ align: 'left' })"
      >
        <i class="icon icon-nav icsz-16"></i>
      </button>

      <button
        class="menubar__button btn btn-icon medium"
        @click="commands.aligntext({ align: 'center' })"
      >
        <i class="icon icon-reorder icsz-16"></i>
      </button>

      <image-upload-partial @select-file="logFile"></image-upload-partial>
      <button @click="apply(commands.image)">Apply</button>
    </div>
  </editor-menu-bar>
</template>

<script>
import { Editor, EditorMenuBar } from 'tiptap'
import ImageUploadPartial from '../partials/image-upload.vue'

export default {
  name: 'MenuBar',
  data() {
    return {
      fileList: [],
    }
  },
  components: {
    EditorMenuBar,
    ImageUploadPartial,
  },
  props: {
    editor: Editor,
  },
  methods: {
    logFile(fileList) {
      this.fileList = [...this.fileList, ...fileList]
    },
    async apply(command) {
      const formData = new FormData()
      this.fileList.forEach((file) => {
        console.log(file)
        formData.append('files', file)
      })
      formData.append('path', '/test')
      try {
        const { data } = await this.$services.file.uploadFiles(formData)
        console.log(data)
        data.forEach((image) => {
          command({ src: image.fileUrl })
        })
      } catch (e) {
        console.log(e)
      }
    },
  },
}
</script>

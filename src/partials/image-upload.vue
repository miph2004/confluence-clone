<template>
  <div class="image-upload-area">
    <input id="upload-input" type="file" accept="image/*" />   
  </div>
</template>

<script>
export default {
  name: "ImageUploadPartial",
  mounted() {
    const imageInput = this.$el.querySelector('#upload-input')
    imageInput.addEventListener('change', e => {
      if(e.target) {
        this.filesSelected(e.target.files)
        e.target.value = ''
      }
    })
  },
  methods: {
    filesSelected(files) {
      [...files].forEach(file=> {
        const reader = new FileReader()
        reader.addEventListener('load', readerEvent => {
          this.$emit('select-file', {
            src: readerEvent.target.result.toString(),
            alt: file.name
          })
          console.log(readerEvent.target.result.toString())
        })
        reader.readAsDataURL(file)
      })
    },
  }
}
</script>

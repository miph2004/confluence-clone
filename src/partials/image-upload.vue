<template>
  <div class="image-upload-area">
    <input id="upload-input" type="file" accept="image/*" />
  </div>
</template>

<script>
export default {
  name: 'ImageUploadPartial',
  mounted() {
    const imageInput = this.$el.querySelector('#upload-input')
    imageInput.addEventListener('change', (e) => {
      if (e.target) {
        this.$emit('select-file', e.target.files)
        e.target.value = ''
      }
    })
  },
}
</script>

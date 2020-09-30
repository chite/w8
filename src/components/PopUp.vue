<template src="./popUp.html"></template>
<script>
export default {
  name: 'PopUp',
  props: {
    open: Boolean,
    viewType: String,
  },
  data(){
      return{
        folderName: '',
        fileName: '',
        file: null
      }
  },
  methods: {
    close() {
      this.folderName = ''
      this.fileName = ''
      this.file = null;
      this.$emit('close-pop-up')
    },
    triggerUpload(){
      this.$refs.fileInput.click()
    },
    handleDrop(e){
      e.preventDefault()
      let file = e.dataTransfer.files[0]
      this.uploaded(file)
    },
    handleUpload(e){
      let file =  e.target.files[0]
      this.uploaded(file)
    },
    createFolder(){
      // emit
      this.$emit('new-folder', this.folderName)
      this.close()
    },
    uploaded(file){
      let reader = new FileReader()
      reader.readAsDataURL(file)
      reader.onload = () =>{
        this.file = reader.result
        this.fileName = file.name
        this.$emit('new-file', this.fileName, this.file)
        this.close()
      }
    }
  },
}
</script>
<style lang="scss" src="./popUp.scss"></style>

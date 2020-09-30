<template src="./home.html" />

<script>
// @ is an alias to /src
import PopUp from '@/components/PopUp.vue'

export default {
  name: 'Home',
  components: {
    PopUp,
  },
  data() {
    return {
      open: false,
      viewType: 'addFile', // addFolder: 新增資料夾，addFile: 新增檔案
      path: [],
      message: false,
      filter: false,
      folders: {
        name: 'Lauren的資料夾',
        star: false,
        data: [
          {
            name: '第一關 - 番茄鐘',
            star: true,
            info: '使用 Vue 和 TypeScript 製作的番茄鐘網頁',
            link: 'https://chite.github.io/w1/dist/#/',
          },
          {
            name: '第三關 - MP3 Player',
            star: true,
            info: '使用 React 和 TypeScript 製作的線上音樂播放器',
            link: 'https://chite.github.io/w3/public/',
          },
          {
            name: '第四關 - 線上支付',
            star: true,
            info: '使用 React 搭配 Hot Module Replacement 開發的應用',
            link: 'https://chite.github.io/w4/public/',
          },
          {
            name: '第六關 - 旅館預約服務',
            star: true,
            info: '使用 Vue 開發',
            link: 'https://chite.github.io/w6/dist/index.html#/',
          },
          {
            name: '第七關 - 匿名聊天室',
            star: true,
            info: '使用 React 和 Node.js 的 socket.io，架設於 Heroku',
            link: 'https://w7-chatroom.herokuapp.com/#/',
          },
          {
            name: '第八關 - 雲端硬碟',
            star: true,
            info: '使用 Vue 3 開發',
            link: 'https://chite.github.io/w8/dist/index.html#/',
          },
          {
            name: '第九關 - 筆記軟體',
            star: true,
            info: '使用 Angular 開發',
            link: 'https://chite.github.io/w9/dist/w9/',
          },
        ],
        subFolders: [
          {
            name: 'ban',
            star: false,
            data: [
              {
                name: 'bash',
                star: false,
              },
              {
                name: 'dict',
                star: false,
              },
            ],
            subFolders: [
              {
                name: 'con',
                star: false,
                data: [
                  {
                    name: 'hash',
                  },
                ],
                subFolders: [],
              },
            ],
          },
          {
            name: 'pics',
            star: false,
            data: [],
            subFolders: [],
          },
          {
            name: 'to do',
            star: false,
            data: [],
            subFolders: [],
          },
          {
            name: 'travel',
            star: false,
            data: [],
            subFolders: [],
          },
        ],
      },
    }
  },
  watch: {
    $route(to) {
      if (to.query.path) {
        this.path = to.query.path.split(',')
      } else {
        this.path = []
      }
    },
  },
  computed: {
    getPath() {
      let counter = 0
      let pointer = this.folders
      while (counter < this.path.length) {
        let index = this.path[counter] // 第幾個子資料夾
        pointer = pointer.subFolders[index]
        counter++
      }
      return pointer
    },
    getFolders() {
      if (!this.filter) return this.getPath.subFolders
      return this.getPath.subFolders.filter((folder) => folder.star)
    },
    getFiles() {
      if (!this.filter) return this.getPath.data
      return this.getPath.data.filter((item) => item.star)
    },
    getParentsPath() {
      let pathArr = []
      let counter = 0
      let pointer = this.folders

      pathArr.push({
        name: pointer.name,
        path: '',
      })
      while (counter < this.path.length) {
        let index = this.path[counter] // 第幾個子資料夾
        let path = this.path.slice(0, counter + 1).join(',')
        pointer = pointer.subFolders[index]
        let name = pointer.name
        pathArr.push({ name, path })
        counter++
      }
      return pathArr
    },
  },
  methods: {
    handleClose() {
      this.open = false
    },
    handleOpen(e, type) {
      if (type == 'folder') {
        this.viewType = 'addFolder'
      }
      if (type == 'file') {
        this.viewType = 'addFile'
      }
      this.open = true
    },
    handleFilter() {
      this.filter = !this.filter
    },
    handleHover(e) {
      let dom = e.target.querySelector("h3[class $='__message']")
      if (dom) dom.style.display = 'block'
      e.target.querySelector("span[class $='__star']").style.display = 'block'
    },
    handleLeave(e) {
      let dom = e.target.querySelector("h3[class $='__message']")
      if (dom) dom.style.display = 'none'
      e.target.querySelector("span[class $='__star']").style.display = 'none'
    },
    enterFolder(e, i) {
      this.path.push(i)
      this.$router.push(
        window.location.origin + window.location.pathname + '?path=' + this.path
      )
    },
    handleStar(e, type, i) {
      e.stopPropagation()
      const path = this.getPath
      if (type == 'file') {
        path.data[i].star = !path.data[i].star
      }
      if (type == 'folder') {
        path.subFolders[i].star = !path.subFolders[i].star
      }
    },
    addFolder(name) {
      this.getPath.subFolders.push({
        name,
        star: false,
        data: [],
        subFolders: [],
      })
    },
    addFile(name, link) {
      this.getPath.data.push({
        name,
        link,
        star: false,
      })
    },
  },
  mounted() {
    if (this.$route.query.path) this.path = this.$route.query.path.split(',')
  },
}
</script>
<style lang="scss" scoped src="./home.scss"></style>

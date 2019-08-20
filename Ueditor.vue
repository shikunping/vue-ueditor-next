<template>
  <div>
    <script :id="randomId" type="text/plain" />
  </div>
</template>

<script>
if (process.client) {
  // require('../../static/UEditor/ueditor.config.js')
  // require('../../static/UEditor/ueditor.all.js')
  // require('../../static/UEditor/lang/zh-cn/zh-cn.js')
  // require('../../static/UEditor/ueditor.parse.js')
  require('./UEditor/ueditor.config.js')
  require('./UEditor/ueditor.all.js')
  require('./UEditor/lang/zh-cn/zh-cn.js')
  require('./UEditor/ueditor.parse.js')
}
export default {
  name: 'VueUEditor',
  props: {
    defaultmsg: {
      type: String,
      default: ''
    },
    config: {
      type: Object,
      default: function () {
        return {
          autoFloatEnabled: false,
          initialFrameWidth: '100%'
        }
      }
    }
  },
  data () {
    return {
      randomId: 'editor_' + (Math.random() * 100000000000000000),
      instance: null
    }
  },
  mounted () {
    // 初始化UE
    // const _this = this
    // this.editor = window.UE.getEditor('editor', _this.config)
    this.initEditor()
  },
  destoryed () {
    // this.editor.destory()
  },
  methods: {
    initEditor () {
      // const _this = this
      // dom元素已经挂载上去了
      this.$nextTick(() => {
        this.instance = window.UE.getEditor(this.randomId, this.config)
        // 绑定事件，当 UEditor 初始化完成后，将编辑器实例通过自定义的 ready 事件交出去
        this.instance.addListener('ready', () => {
          this.ready = true
          this.$emit('ready', this.instance)
        })
      })
    },
    setText (con) {
      this.instance = window.UE.getEditor(this.randomId, this.config)
      this.instance.setContent(con)
    },
    getUEContent: function () {
      return this.editor.getContent()
    },
    destoryed () {
      if (this.instance !== null && this.instance.destroy) {
        this.instance.destroy()
      }
      // this.editor.destory()
    }
  }
}
</script>

<style>

</style>

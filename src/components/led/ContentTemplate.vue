<template>
  <el-dropdown ref="dropdown" menu-align="start" trigger="click" :hide-on-click="false" class="content-template__page">
    <el-tooltip
      :content="tips"
      :disabled="!tips"
      placement="top-start">
      <div class="el-textarea content-template">
        <textarea
          ref="textarea"
          class="el-textarea__inner"
          :value="parseValue"
          :maxlength="maxlength"
          @click.stop="active"
          @select="active"
          @keyup="active"
          @input="handleInput"
          @blur="handleBlur"
          @keydown="handleKeyDown"
          @paste="onPaste">
        </textarea>
      </div>
    </el-tooltip>
    <el-dropdown-menu slot="dropdown" style="width:520px;padding:0 5px;">
      <el-button
        style="margin:4px;"
        v-for="item in options"
        size="small"
        :key="item.code"
        :disabled="selectDisabled || codeMatchs.includes(item.code) || disabledMatchs.includes(item.code)"
        @click="handleClick(item)">
        {{item.name}}
      </el-button>
    </el-dropdown-menu>
  </el-dropdown>
</template>

<script>
  import { cursorBetween, cursorBefore, cursorAfter, getCursorPosition, setCursorPosition, insertAtCaret, cursorMoveToEnd } from '@/utils/textField'
  import emitter from 'hui/lib/mixins/emitter'

  export default {
    name: 'ContentTemplate',

    mixins: [emitter],

    props: {
      value: {
        type: String,
        required: true
      },

      options: {
        type: Array,
        required: true
      },

      multipleLimit: {
        type: Number,
        default: 0
      },

      startFlag: {
        type: String,
        default: '['
      },

      endFlag: {
        type: String,
        default: ']'
      },

      disabledMatchs: {
        type: Array,
        default: () => []
      },

      tips: {
        type: String,
        default: ''
      },

      maxlength: {
        type: Number,
        default: 64
      }
    },

    data () {
      return {
        visible: false,
        currentCursorPosition: null
      }
    },

    computed: {
      /**
       * @description 输入框内的文本与value实现双向绑定转换
       * @author zhengjunling
       */
      parseValue: {
        get () {
          const { startFlag, endFlag } = this
          let text = this.value
          this.codeMatchs.forEach(match => {
            const reg = new RegExp(`\\${startFlag}${match}\\${endFlag}`, 'g')
            const code = this.getNameByCode(match)
            text = text.replace(reg, `${startFlag}${code}${endFlag}`)
          })
          return text
        },
        set (val) {
          const { startFlag, endFlag } = this
          let text = val
          this.getMatchs(val).forEach(match => {
            const reg = new RegExp(`\\${startFlag}${match}\\${endFlag}`, 'g')
            const name = this.getCodeByName(match)
            text = text.replace(reg, `${startFlag}${name}${endFlag}`)
          })
          this.$emit('input', text)
          this.$emit('contentChange')
        }
      },

      codeMatchs () {
        return this.getMatchs(this.value)
      },

      selectDisabled () {
        if (this.multipleLimit === 0) return false
        else {
          return this.codeMatchs.length >= this.multipleLimit
        }
      }
    },

    methods: {
      /**
       * @description 获取匹配模板格式的子串
       * @author zhengjunling
       */
      getMatchs (val) {
        const { startFlag, endFlag } = this
        const reg = new RegExp(`\\${startFlag}[^\\${startFlag}\\${endFlag}]*(?=${endFlag})`, 'g')
        const matchs = val.match(reg)
        if (matchs) {
          return matchs.map(item => {
            return item.replace(startFlag, '')
          })
        } else {
          return []
        }
      },

      /**
       * @description 通过内容获取对应的code
       * @author zhengjunling
       */
      getNameByCode (code) {
        const option = this.options.find(option => {
          return option.code === code
        })

        return option && option.name
      },

      /**
       * @description 通过code获取对应的内容
       * @author zhengjunling
       */
      getCodeByName (name) {
        const option = this.options.find(option => {
          return option.name === name
        })

        return option && option.code
      },

      /**
       * @description 激活下拉框并重新计算光标位置
       * @author zhengjunling
       */
      active () {
        this.currentCursorPosition = getCursorPosition(this.$refs.textarea)
        this.$refs.dropdown.visible = true
      },

      /**
       * @description 输入框值改变
       * @author zhengjunling
       */
      handleInput (event) {
        this.parseValue = event.target.value.replace('\n', '')
      },

      /**
       * @description 输入框失去焦点时触发form表单校验
       * @author zhengjunling
       */
      handleBlur () {
        this.dispatch('ElFormItem', 'el.form.blur', [this.parseValue])
      },

      /**
       * @description 点击下拉选项
       * @author zhengjunling
       */
      handleCommand (command) {
        const { $refs, currentCursorPosition, startFlag, endFlag } = this
        const insertText = `${startFlag}${command}${endFlag}`
        const between = cursorBetween($refs.textarea, currentCursorPosition, startFlag, endFlag)
        if (between) {
          this.parseValue = this.parseValue + insertText
        } else {
          this.parseValue = insertAtCaret($refs.textarea, currentCursorPosition, insertText)
        }
      },

      handleClick (item) {
        const { $refs, currentCursorPosition, startFlag, endFlag } = this
        const insertText = `${startFlag}${item.name}${endFlag}`
        const between = cursorBetween($refs.textarea, currentCursorPosition, startFlag, endFlag)
        // 当内容长度超过maxlength设置的值的时候，不允许再添加
        if (this.maxlength && ($refs.textarea.value + insertText).length > this.maxlength) return
        if (between) {
          this.parseValue = this.parseValue + insertText
        } else {
          this.parseValue = insertAtCaret($refs.textarea, currentCursorPosition, insertText)
        }
      },

      /**
       * @description 键盘事件
       * @author zhengjunling
       */
      handleKeyDown (ev) {
        const { parseValue, startFlag, endFlag } = this
        const { textarea } = this.$refs
        const keyCode = ev.keyCode
        const key = ev.key
        const currentCursorPosition = getCursorPosition(textarea)
        const between = cursorBetween(textarea, currentCursorPosition, startFlag, endFlag)
        const before = cursorBefore(textarea, currentCursorPosition, startFlag, endFlag)
        const after = cursorAfter(textarea, currentCursorPosition, startFlag, endFlag)

        if (between) {
          if (keyCode === 8 || keyCode === 46) {
            // 删除按键将删除当前光标所处位置的模板
            this.removeTemplate(parseValue.slice(0, between.start) + parseValue.slice(between.end), between.start)
            ev.preventDefault()
          } else if (keyCode > 36 && keyCode < 41) {
            // 上下左右箭头不做任何处理
            return
          } else if (keyCode === 13 || keyCode === 32 || (keyCode >= 48 && keyCode <= 111) || keyCode >= 186) {
            // 在模板中间输入内容时，自动将鼠标移至末尾
            cursorMoveToEnd(textarea)
            ev.preventDefault()
            return
          }
          // 其他按键阻止默认行为
          ev.preventDefault()
        } else if (after && keyCode === 8) {
          // 鼠标位于模板后面，且当前按下回退键将删除光标前的模板
          this.removeTemplate(parseValue.slice(0, after.start) + parseValue.slice(after.end), after.start)
          ev.preventDefault()
        } else if (before && keyCode === 46) {
          // 鼠标位于模板前面，且当前按下delete键将删除光标后的模板
          this.removeTemplate(parseValue.slice(0, before.start) + parseValue.slice(before.end), before.start)
          ev.preventDefault()
        } else if (key === '[' || key === ']' || key === '^' || key === '$' || key === 'Enter') {
          ev.preventDefault()
        }
      },

      /**
       * @Author: liumeng6
       * @Date: 2018-06-22 13:35:30
       * @Desc: 复制事件
       */
      onPaste (event) {
        // `ie`和`chrome`兼容
        const pasteText = event.clipboardData ? event.clipboardData.getData('text/plain').trim() : window.clipboardData.getData('text').trim()
        if (pasteText.includes('[') || pasteText.includes(']') || pasteText.includes('^') || pasteText.includes('$')) {
          event.preventDefault()
        }
      },

      /**
       * @description 删除模板
       * @author zhengjunling
       */
      removeTemplate (newValue, newCursorPosition) {
        this.currentCursorPosition = newCursorPosition
        this.parseValue = newValue
        this.$nextTick(() => {
          setCursorPosition(this.$refs.textarea, newCursorPosition)
        })
      }
    }
  }
</script>

<style lang="less">
.content-template__page {
  width: 100%;
  .content-template {
    width: 100%;
  }
  .el-button {
    font-weight: normal !important;
    /* span {
      font-weight: normal !important;
    } */
  }
  .el-button, .el-cascader-menu__item__keyword {
    font-weight: normal !important;
  }
}
.text-area-noborder {
  .el-textarea__inner {
    border: none;
  }
}

</style>

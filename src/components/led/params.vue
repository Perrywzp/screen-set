/*
 * @Author: wangzhipei
 * @Date: 2018-12-07 10:48:52
 * @Desc: led屏配置， 配置参数模块
 * -----
 * @Last Modified By: wangzhipei
 * @Last Modified: 2018-12-07 10:48:52
 */

<template>
  <table class="led-params" border="1">
    <tr>
      <td width="90" align="center">文字大小</td>
      <td>
        <el-select v-model="value.fontSize">
          <el-option v-for="item in options.fontSizes" :value="item.value" :key="item.value" :label="item.name">{{item.name}}</el-option>
        </el-select>
      </td>
    </tr>
    <tr>
      <td align="center">文字颜色</td>
      <td>
        <ul class="btn-radio">
          <li v-for="item in options.fontColors" :key="item.value"
            :class="{active: item.value === value.fontColor}"
            @click="value.fontColor=item.value">
          <span>{{item.name}}</span></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td align="center">显示方式</td>
      <td>
        <ul class="btn-radio">
          <li v-for="item in options.showModes" :key="item.value"
            :class="{active: item.value === value.showMode}"
            @click="value.showMode=item.value">
          <span>{{item.name}}</span></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td align="center">对其方式</td>
      <td>
        <ul class="btn-radio">
          <li v-for="item in options.horAligns" :key="item.value"
            :class="{active: item.value === value.horAlign}"
            @click="value.horAlign=item.value">
          <span>{{item.name}}</span></li>
        </ul>
      </td>
    </tr>
    <tr>
      <td align="center">是否加粗</td>
      <td>
         <el-radio class="radio" v-model="value.fontWeight" v-for="(item, index) in options.fontWeights" :label="item.value" :key="index">{{item.name}}</el-radio>
      </td>
    </tr>
    <tr>
      <td align="center">显示类型</td>
      <td>
        <el-select v-model="displayContents">
          <el-option  v-for="(item, index) in displayKeywords" :value="item.childrens" :key="index" :label="item.name">{{item.name}}</el-option>
        </el-select>
        </td>
    </tr>
    <tr>
      <td align="center">显示内容</td>
      <td>
        <el-dropdown trigger="click" :hide-on-click="false" @command="hdKeywordClick">
          <el-tooltip
            :content="tips"
            :disabled="!tips"
            placement="top-start">
            <textarea
              ref="textarea"
              :rows="2"
              placeholder="请输入内容"
              @keydown="handleKeyDown" v-model="value.text"></textarea>
          </el-tooltip>
          <el-dropdown-menu slot="dropdown" class="led-params-keywords">
            <el-dropdown-item v-for="(item, index) in displayContents" :key="index" :command="item" :disabled="boolActive(item)">{{item.name}}</el-dropdown-item>
          </el-dropdown-menu>
         </el-dropdown>
      </td>
    </tr>
  </table>
</template>
<script>
export default {
  name: 'led-params',
  props: {
    value: {
      type: Object,
      default () {
        return {
          fontColor: '#ff0000',
          fontSize: 16,
          showMode: 0,
          fontWeight: 'normal',
          horAlign: 'left'
        }
      }
    },
    displayKeywords: {
      type: Array,
      default () {
        return [
          {
            name: '类型一',
            id: '11111',
            childrens: [
              {name: '关键字1', id: 'key1111'},
              {name: '关键字2', id: 'key2221'}
            ]
          },
          {
            name: '类型二',
            id: '11111',
            childrens: [
              {name: '关键字3', id: 'key3333'},
              {name: '关键字4', id: 'key4444'}
            ]
          }
        ]
      }
    }
  },
  data () {
    return {
      displayContents: [],
      tips: '1～64个字符；不能包含 [ ] ^ $ 这些特殊字符。',
      options: {
        fontSizes: [
          {name: '16px', value: 16},
          {name: '24px', value: 24},
          {name: '36px', value: 36}
        ],
        fontColors: [
          {name: '红色', value: '#ff0000'},
          {name: '黄色', value: '#ffff00'},
          {name: '绿色', value: '#00ff00'}
        ],
        showModes: [
          {name: '静止', value: 1},
          {name: '向上滚动', value: 4},
          {name: '向下滚动', value: 5},
          {name: '向左滚动', value: 2},
          {name: '向右滚动', value: 3}
        ],
        horAligns: [
          {name: '居左', value: 0, icon: ''},
          {name: '居中', value: 1, icon: ''},
          {name: '居右', value: 2, icon: ''}
        ],
        fontWeights: [
          {name: '是', value: 'bold'},
          {name: '否', value: 'normal'}
        ]
      },
      selectKeywords: []
    }
  },
  watch: {
    selectKeywords: {
      handle () {
        // this.value.text += this.selectKeywords
      },
      deep: true
    }
  },
  methods: {
    hdKeywordClick (item) {
      !this.selectKeywords.some(obj => obj.id === item.id) && this.selectKeywords.push(item)
    },
    boolActive (item) {
      return this.selectKeywords.some(val => val.id === item.id)
    },
    handleKeyDown (ev) {
      console.log(ev)
      // const { parseValue, startFlag, endFlag } = this
      const { textarea } = this.$refs
      console.log(textarea)
      // const keyCode = ev.keyCode
      // const key = ev.key
      // const currentCursorPosition = getCursorPosition(textarea)
      // const between = cursorBetween(textarea, currentCursorPosition, startFlag, endFlag)
      // const before = cursorBefore(textarea, currentCursorPosition, startFlag, endFlag)
      // const after = cursorAfter(textarea, currentCursorPosition, startFlag, endFlag)

      // if (between) {
      //   if (keyCode === 8 || keyCode === 46) {
      //     // 删除按键将删除当前光标所处位置的模板
      //     this.removeTemplate(parseValue.slice(0, between.start) + parseValue.slice(between.end), between.start)
      //     ev.preventDefault()
      //   } else if (keyCode > 36 && keyCode < 41) {
      //     // 上下左右箭头不做任何处理
      //     return
      //   } else if (keyCode === 13 || keyCode === 32 || (keyCode >= 48 && keyCode <= 111) || keyCode >= 186) {
      //     // 在模板中间输入内容时，自动将鼠标移至末尾
      //     // cursorMoveToEnd(textarea)
      //     ev.preventDefault()
      //     return
      //   }
      //   // 其他按键阻止默认行为
      //   ev.preventDefault()
      // } else if (after && keyCode === 8) {
      //   // 鼠标位于模板后面，且当前按下回退键将删除光标前的模板
      //   this.removeTemplate(parseValue.slice(0, after.start) + parseValue.slice(after.end), after.start)
      //   ev.preventDefault()
      // } else if (before && keyCode === 46) {
      //   // 鼠标位于模板前面，且当前按下delete键将删除光标后的模板
      //   this.removeTemplate(parseValue.slice(0, before.start) + parseValue.slice(before.end), before.start)
      //   ev.preventDefault()
      // } else if (key === '[' || key === ']' || key === '^' || key === '$' || key === 'Enter') {
      //   ev.preventDefault()
      // }
    }
  }
}
</script>
<style lang="less" scoped>
  .led-params {
    width: 100%;
    height: 100%;
    border-spacing:0;
    border-collapse:collapse;
    tr, td {
      border: 1px solid #ccc;
    }
    td {
      padding: 5px 7px;
      .el-dropdown{
        width: 100%;
      }
      textarea {
        border: none !important;
      }
      .btn-radio {
        display: block;
        list-style: none;
        margin: 0;
        padding: 0;
        li {
          cursor: pointer;
          float: left;
          width: 75px;
          height: 25px;
          line-height: 25px;
          text-align: center;
          border: 1px solid #ccc;
          margin-right: -1px;
          &.active span{
            margin-top: -1px;
            border: 1px solid #1E7FFF;
            color: #1E7FFF;
          }
          span {
            display: block;
          }
        }
      }
    }
    tr td:first-child {
      background: #eee;
      padding: 5px 13px;
    }
  }
</style>
<style lang="less">
table.led-params {
  textarea {
    border: none !important;
    padding: 0;
    resize: none;
    width: 100%;
  }
}
.el-dropdown-menu.led-params-keywords {
  width: 420px;
  padding: 5px 12px 0 12px;
  overflow: hidden;
  box-sizing: border-box;
  li {
    float: left;
    border: 1px solid #ccc;
    border-radius: 3px;
    padding: 0 8px;
    height: 28px;
    line-height: 28px;
    margin: 0 12px 5px 0;
  }
}
</style>

/*
 * @Author: wangzhipei
 * @Date: 2018-12-04 07:02:59
 * @Desc: 用于led屏自定义区域
 * -----
 * @Last Modified By: wangzhipei
 * @Last Modified: 2018-12-04 07:02:59
 */
<template>
  <div class='led-set'>
    <toolbar v-model="screenSize" @del="hdDel"></toolbar>
    <div class="led-box">
      <div class="screen-layout" :style="{width: layout.width + 'px', height: layout.height + 'px'}">
        <screen v-model="value" @selected="selected"></screen>
      </div>
      <params v-show="curIndex !== ''" v-model="curRect" class="led-params"></params>
    </div>
  </div>
</template>
<script>
import toolbar from './toolbar'
import screen from './screen'
import params from './params'
export default {
  name: 'led-set',
  components: { toolbar, params, screen },
  props: {
    layout: {
      type: Object,
      default () {
        return {
          width: 420,
          height: 420
        }
      }
    },
    value: {
      type: Array,
      default () {
        return [{
          x: 20,
          y: 20,
          width: 300,
          height: 75,
          text: '恭喜发财',
          fontColor: '#c2484c',
          fontSize: 16,
          fontWeight: 'normal',
          horAlign: 'left'
        }, {
          x: 20,
          y: 120,
          width: 350,
          height: 75,
          text: '红包拿来',
          fontColor: '#e5de1d',
          fontSize: 18,
          fontWeight: 'normal',
          horAlign: 'center'
        }, {
          x: 20,
          y: 220,
          width: 400,
          height: 75,
          text: '城阙辅三秦，风烟望五津, 与君离别意，同是宦游人; 海内存知己，天涯若比邻; 无为在歧路，儿女共沾巾',
          fontColor: '#1de51d',
          fontSize: 20,
          fontWeight: 'bold',
          horAlign: 'right'
        }]
      }
    }
  },
  data () {
    return {
      curRect: {},
      curIndex: '',
      screenSize: {
        width: 420,
        height: 420
      }
    }
  },
  mounted () {
  },
  watch: {
    curRect () {
      this.value[this.curIndex] = this.curRect
    }
  },
  methods: {
    selected ({index, rect}) {
      this.curRect = rect
      this.curIndex = index
    },
    hdDel () {
      this.$confirm('此操作将永久删除该区域数据, 是否继续?', '提示', {
        confirmButtonText: '确定',
        cancelButtonText: '取消',
        type: 'question'
      }).then(() => {
        this.value.splice(this.curIndex, 1)
        this.curIndex = ''
        this.$message({
          type: 'success',
          message: '删除成功!'
        })
      }).catch(() => {})
    }
  }
}
</script>
<style lang='less' scoped>
.led-set {
  width: 1024px;
  height: 600px;
}
.led-box {
  width: 100%;
  .screen-layout{
    float: left;
    margin-right: 20px;
  }
  .led-params {
    width: 54%;
    height: 100%;
  }
}
</style>

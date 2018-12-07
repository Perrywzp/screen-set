/*
 * @Author: wangzhipei
 * @Date: 2018-12-07 10:49:10
 * @Desc: led配置， 工具条模块
 * -----
 * @Last Modified By: wangzhipei
 * @Last Modified: 2018-12-07 10:49:10
 */

<template>
  <div class="led-toolbar">
    <span v-popover:popover><i :class="options.iconsClass.set"></i>配置屏幕尺寸</span>
    <span @click="del"><i :class="options.iconsClass.del"></i>删除</span>
    <em>{{layoutDesc}}</em>
    <el-popover
      ref="popover"
      placement="bottom"
      width="300"
      v-model="setFormVisible"
      trigger="click">
      <el-form ref="form" :model="formVal" :rules="rules" label-width="52px" style="width: 260px;margin: 0 auto;">
        <el-form-item label="屏宽" prop="width">
          <el-input v-model="formVal.width" name="width"><template slot="append">像素</template></el-input>
        </el-form-item>
        <el-form-item label="屏高" prop="height">
          <el-input v-model="formVal.height" name="height"><template slot="append">像素</template></el-input>
        </el-form-item>
      </el-form>
      <div style="float: right;">
        <el-button type="primary"  @click="confirm">确定</el-button>
        <el-button @click="setFormVisible = false">取消</el-button>
      </div>
  </el-popover>
  </div>
</template>
<script>
export default {
  name: 'led-toolbar',
  props: {
    options: {
      type: Object,
      default () {
        return {
          iconsClass: {
            set: 'el-icon-setting',
            del: 'el-icon-delete'
          }
        }
      }
    },
    value: {
      type: Object,
      default () {
        return {
          width: '',
          height: ''
        }
      }
    }
  },
  data () {
    return {
      layoutDesc: '',
      setFormVisible: false,
      formVal: this.value,
      rules: {
        width: [{required: true, message: '请输入屏宽', trigger: 'blur'}],
        height: [{required: true, message: '请输入屏高', trigger: 'blur'}]
      }
    }
  },
  methods: {
    confirm () {
      this.setFormVisible = false
      this.value = this.formVal
    },
    del () {
      this.$emit('del')
    }
  }
}
</script>
<style lang="less" scoped>
.led-toolbar {
  height: 36px;
  line-height: 36px;
  text-indent: 3px;
  span {
    cursor: pointer;
    margin-right: 15px;
    i {
      margin-right: 8px;
    }
  }
  .set-form{
    width: 200px;
  }
}

</style>

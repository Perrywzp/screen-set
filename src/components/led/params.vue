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
          <li v-for="item in options.fontColors" :value="item.value" :key="item.value"
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
          <li v-for="item in options.showModes" :value="item.value" :key="item.value"
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
          <li v-for="item in options.horAligns" :value="item.value" :key="item.value"
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
      <td> 【】 </td>
    </tr>
    <tr>
      <td align="center">显示内容</td>
      <td> 【】 </td>
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
    }
  },
  data () {
    return {
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
      }
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
      padding: 7px 13px;
      .btn-radio {
        display: block;
        list-style: none;
        margin: 0;
        padding: 0;
        li {
          cursor: pointer;
          float: left;
          width: 75px;
          height: 30px;
          line-height: 30px;
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
    }
  }
</style>

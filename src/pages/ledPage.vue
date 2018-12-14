<template>
  <div>
    <led v-model="data" :layout="layout" :options="options"></led>
  </div>
</template>
<script>
import led from '@/components/led/index'
export default {
  name: 'ledPage',
  components: { led },
  data () {
    return {
      options: {multipleLimit: 3, placeholder: ['[', ']']},
      layout: {width: 420, height: 420},
      data: {
        size: {width: 488, height: 248},
        areas: [{
          x: 20,
          y: 20,
          width: 300,
          height: 75,
          text: '恭喜发财',
          fontColor: '#ff0000',
          fontSize: 16,
          fontWeight: 'normal',
          showMode: 1,
          horAlign: 0
        }, {
          x: 20,
          y: 120,
          width: 350,
          height: 75,
          text: '红包拿来',
          fontColor: '#ffff00',
          fontSize: 24,
          fontWeight: 'normal',
          showMode: 2,
          horAlign: 1
        }, {
          x: 20,
          y: 220,
          width: 380,
          height: 75,
          text: '城阙辅三秦，风烟望五津, 与君离别意，同是宦游人; 海内存知己，天涯若比邻; 无为在歧路，儿女共沾巾',
          fontColor: '#00ff00',
          fontSize: 36,
          fontWeight: 'bold',
          showMode: 4,
          horAlign: 2
        }]
      }
    }
  },
  mounted () {},
  methods: {
  }
}
</script>

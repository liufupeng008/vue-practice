<template>
  <div class="Plan">
    <el-radio-group v-model="radio1">
      <el-radio-button label="上海"></el-radio-button>
      <el-radio-button label="北京"></el-radio-button>
      <el-radio-button label="广州"></el-radio-button>
      <el-radio-button label="深圳"></el-radio-button>
    </el-radio-group>

    <el-form style="margin-top=10px;">
      <el-form-item label="名称">
        <my-calendar></my-calendar>
      </el-form-item>
    </el-form>
  </div>
</template>

<i18n>
</i18n>

<script>
import MyCalendar from '../components/MyCalendar'

export default {
  name: 'Plan',

  components: {
    MyCalendar
  },

  props: {},

  data () {
    return {
      radio1: '上海'
    }
  },

  computed: {},

  watch: {},

  // mounted() {
  // this.init()
  // },

  methods: {
  }
}
</script>

<style scoped>
</style>

<template>
  <section>
    <button @click="handlePreMonth">上一月</button>
    <button @click="handleNextMonth">下一月</button>
    <ul>
      <li
        v-for="(item, index) in days"
        :key="index"
      >
        {{item.day}}
      </li>
    </ul>
  </section>
</template>

<script>
export default {
  computed: {
  },
  data () {
    return {
      currentDay: 1, //  初始化日
      currentMonth: 1, // 初始化月
      currentYear: 1970, // 初始化年
      currentWeek: 1, // 周
      days: [] // 当月数据存储
    }
  },
  methods: {
    initData (nowDay) { // 初始化数据
      let date = ''
      if (nowDay) {
        date = new Date(nowDay)
      } else {
        let now = new Date()
        let d = new Date(this.formDateSet(now.getFullYear(), now.getMonth(), 1))
        // alert(d.getDate())
        d.setDate(35)
        date = new Date(this.formDateSet(d.getFullYear(), d.getMonth() + 1, 1))
      }
      this.currentDay = date.getDate()
      this.currentMonth = date.getMonth() + 1
      this.currentYear = date.getFullYear()
      this.currentWeek = date.getDay() === 0 ? 7 : date.getDay()
      this.days.length = 0
      let daySet = this.formDateSet(this.currentYear, this.currentMonth, this.currentDay)
      for (let i = this.currentWeek - 1; i >= 0; i--) {
        let dayTwo = new Date(daySet)
        dayTwo.setDate(dayTwo.getDate() - i)
        let obj = {}
        obj.day = dayTwo
        this.days.push(obj)
      }

      for (let i = 1; i <= 35 - this.currentWeek; i++) {
        let d = new Date(daySet)
        d.setDate(d.getDate() + i)
        let obj = {}
        obj.day = d
        this.days.push(obj)
      }
      console.log(this.days)
    },
    handlePreMonth () { // 获取前一个月
      let d = new Date(this.formDateSet(this.currentYear, this.currentMonth, 1))
      d.setDate(0)
      this.initData(this.formDateSet(d.getFullYear(), d.getMonth() + 1, 1))
    },
    handleNextMonth () { // 获取下一个月
      let d = new Date(this.formDateSet(this.currentYear, this.currentMonth, 1))
      d.setDate(35)
      this.initData(this.formDateSet(d.getFullYear(), d.getMonth() + 1, 1))
    },
    formDateSet (year, month, day) { // 日期格式化
      let y = year
      let m = month > 10 ? month : '0' + month
      let d = day > 10 ? day : '0' + day
      return `${y}-${m}-${d}`
    }
  },
  beforeDestroy () {
  },
  mounted () {
    this.initData()
  }
}
</script>

<style lang="less" scoped>
</style>

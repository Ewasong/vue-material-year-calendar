<template>
  <div id="app">
    <year-calendar
      v-model="year"
      :startDate="'2019-02-03'"
      :endDate="'2020-05-03'"
      :activeDates.sync="activeDates"
      @toggleDate="toggleDate"
      :lang="lang"
      prefixClass="your_customized_wrapper_class"
      :activeClass="activeClass"
    ></year-calendar>
  </div>
</template>

<script>
import YearCalendar from './components/YearCalendar.vue'
import dayjs from 'dayjs'

export default {
  name: 'app',
  components: {
    YearCalendar
  },
  data () {
    return {
      lang: 'zh', // 'en', 'tw', 'pt'
      year: 2019,
      activeDates: [
        { date: '2019-02-13' },
        { date: '2019-02-14', className: 'red' },
        { date: '2019-02-15', className: 'blue' },
        { date: '2019-02-16', className: 'your_customized_classname' }
      ],
      activeClass: '',
      showYearSelector: true
    }
  },
  methods: {
    toggleDate (dateInfo) {
    },
    add_sat_and_sun_of_year () {
      let theDate = dayjs(`${this.year}-01-01`)

      while (theDate.diff(theDate.endOf('year'), 'day') !== 0) {
        if (theDate.day() === 6 || theDate.day() === 0) {
          // 將週六或週日加入 activeDates 中
          // add weekend to activeDates
          this.activeDates.push(theDate.format('YYYY-MM-DD'))
        }
        theDate = theDate.add(1, 'day')
      }

      // remove duplicate key
      this.activeDates = this.activeDates.filter(function (item, pos, self) {
        return self.indexOf(item) === pos
      }).sort()
    },
    remove_sat_and_sun_of_year () {
      for (let i = this.activeDates.length - 1; i >= 0; i--) {
        let date = this.activeDates[i]
        if (dayjs(date).year() === this.year && (dayjs(date).day() === 6 || dayjs(date).day() === 0)) {
          this.activeDates.splice(i, 1)
        }
      }
    }
  }
}
</script>

<style lang="stylus">
*
  margin 0
  padding 0
  box-sizing border-box
#app
  padding 60px
  background-color #eaeaea
.your_customized_wrapper_class
  background-color: #0aa
  color: white
  &.red
    background-color: #a00
    color: white
    &:after
      background-image url('./assets/baseline-remove_circle-24px.svg')
      background-size 100% 100%
  &.blue
    background-color: #0000aa
    color: white
  &.your_customized_classname
    background-color: yellow
    color: black

</style>

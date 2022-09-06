<script>
export default defineComponent({
  name: 'Vue3CronCore',
  props: {
    maxHeight: String,
    change: Function,
    value: String,
  },
  setup(props, { emit }) {
    const { i18n } = toRefs(props)
    const weekList = ref([
      {
        key: 2,
        value: '星期一',
      },
      {
        key: 3,
        value: '星期二',
      },
      {
        key: 4,
        value: '星期三',
      },
      {
        key: 5,
        value: '星期四',
      },
      {
        key: 6,
        value: '星期五',
      },
      {
        key: 7,
        value: '星期六',
      },
      {
        key: 1,
        value: '星期日',
      },
    ])
    const state = reactive({
      second: {
        cronEvery: '1',
        incrementStart: 3,
        incrementIncrement: 5,
        rangeStart: 0,
        rangeEnd: 0,
        specificSpecific: [],
      },
      minute: {
        cronEvery: '1',
        incrementStart: 3,
        incrementIncrement: 5,
        rangeStart: 0,
        rangeEnd: 0,
        specificSpecific: [],
      },
      hour: {
        cronEvery: '1',
        incrementStart: 3,
        incrementIncrement: 5,
        rangeStart: 0,
        rangeEnd: 0,
        specificSpecific: [],
      },
      day: {
        cronEvery: '1',
        incrementStart: 1,
        incrementIncrement: 1,
        rangeStart: 0,
        rangeEnd: 0,
        specificSpecific: [],
        cronLastSpecificDomDay: 1,
        cronDaysBeforeEomMinus: 0,
        cronDaysNearestWeekday: 1,
        cycle01: 1,
        cycle02: 2,
      },
      week: {
        cronEvery: '1',
        cycle01: 2,
        cycle02: 3,
        average01: 1,
        average02: 2,
        weekday: 2,
        incrementStart: 1,
        incrementIncrement: 1,
        specificSpecific: [],
        cronNthDayDay: 1,
        cronNthDayNth: 1,
      },
      month: {
        cronEvery: '1',
        incrementStart: 3,
        incrementIncrement: 5,
        rangeStart: 1,
        rangeEnd: 1,
        specificSpecific: [],
      },
      year: {
        cronEvery: '1',
        incrementStart: 2022,
        incrementIncrement: 1,
        rangeStart: 1,
        rangeEnd: 1,
        specificSpecific: [],
      },
      output: {
        second: '',
        minute: '',
        hour: '',
        day: '',
        month: '',
        Week: '',
        year: '',
      },
      secondsText: computed(() => {
        let seconds = ''
        const cronEvery = state.second.cronEvery
        switch (cronEvery.toString()) {
          case '1':
            seconds = '*'
            break
          case '2':
            seconds = `${state.second.incrementStart}/${state.second.incrementIncrement}`
            break
          case '3':
            state.second.specificSpecific.map((val) => {
              seconds += `${val},`
            })
            seconds = seconds.slice(0, -1)
            break
          case '4':
            seconds = `${state.second.rangeStart}-${state.second.rangeEnd}`
            break
        }
        return seconds
      }),
      minutesText: computed(() => {
        let minutes = ''
        const cronEvery = state.minute.cronEvery
        switch (cronEvery.toString()) {
          case '1':
            minutes = '*'
            break
          case '2':
            minutes = `${state.minute.incrementStart}/${state.minute.incrementIncrement}`
            break
          case '3':
            state.minute.specificSpecific.map((val) => {
              minutes += `${val},`
            })
            minutes = minutes.slice(0, -1)
            break
          case '4':
            minutes = `${state.minute.rangeStart}-${state.minute.rangeEnd}`
            break
        }
        return minutes
      }),
      hoursText: computed(() => {
        let hours = ''
        const cronEvery = state.hour.cronEvery
        switch (cronEvery.toString()) {
          case '1':
            hours = '*'
            break
          case '2':
            hours = `${state.hour.incrementStart}/${state.hour.incrementIncrement}`
            break
          case '3':
            state.hour.specificSpecific.map((val) => {
              hours += `${val},`
            })
            hours = hours.slice(0, -1)
            break
          case '4':
            hours = `${state.hour.rangeStart}-${state.hour.rangeEnd}`
            break
        }
        return hours
      }),
      daysText: computed(() => {
        let days = ''
        const cronEvery = state.day.cronEvery
        switch (cronEvery.toString()) {
          case '1':
            days = '*'
            break
          case '2':
            days = '?'
            break
          case '3':
            const cycle01 = checkNumber(state.day.cycle01, 1, 30)
            const cycle02 = checkNumber(state.day.cycle02, cycle01 ? cycle01 + 1 : 2, 31, 31)
            days = `${cycle01}-${cycle02}`
            break
          case '4':
            const average01 = checkNumber(state.day.incrementStart, 1, 30)
            const average02 = checkNumber(state.day.incrementIncrement, 1, 31 - average01 || 0)
            days = `${average01}/${average02}`
            break
          case '5':
            days = `${state.day.cronDaysNearestWeekday}W`
            break
          case '6':
            days = 'L'
            break
          case '7':
            // days = 'LW'
            state.day.specificSpecific.map((val) => {
              days += `${val},`
            })
            days = days.slice(0, -1)
            break
          case '8':
            days = `${state.day.cronLastSpecificDomDay}L`
            break
          case '9':
            days = `L-${state.day.cronDaysBeforeEomMinus}`
            break
          case '10':
            days = `${state.day.cronDaysNearestWeekday}W`
            break
          case '12':
            days = '?'
            break
        }
        return days
      }),
      weeksText: computed(() => {
        let weeks = ''
        const cronEvery = state.week.cronEvery
        if (cronEvery.toString() !== '2' && state.day.cronEvery !== '?') {
          // state.daysText = '?'
          // state.day.cronEvery = '2'
        }
        switch (cronEvery.toString()) {
          case '1':
            weeks = '*'
            break
          case '2':
            weeks = '?'
            break
          case '3':
            weeks = state.cycleTotal
            break
          case '4':
            weeks = state.averageTotal
            break
          case '5':
            weeks = `${state.week.weekday}L`
            break
          case '6':
            state.week.specificSpecific.map((val) => {
              weeks += `${val},`
            })
            weeks = weeks.slice(0, -1)
            break
        }
        console.log(`weeks=${weeks}`)
        return weeks
      }),
      monthsText: computed(() => {
        let months = ''
        const cronEvery = state.month.cronEvery
        switch (cronEvery.toString()) {
          case '1':
            months = '*'
            break
          case '2':
            months = `${state.month.incrementStart}/${state.month.incrementIncrement}`
            break
          case '3':
            state.month.specificSpecific.map((val) => {
              months += `${val},`
            })
            months = months.slice(0, -1)
            break
          case '4':
            months = `${state.month.rangeStart}-${state.month.rangeEnd}`
            break
        }
        return months
      }),
      yearsText: computed(() => {
        let years = ''
        const cronEvery = state.year.cronEvery
        switch (cronEvery.toString()) {
          case '1':
            years = '*'
            break
          case '2':
            years = `${state.year.incrementStart}/${state.year.incrementIncrement}`
            break
          case '3':
            state.year.specificSpecific.map((val) => {
              years += `${val},`
            })
            years = years.slice(0, -1)
            break
          case '4':
            years = `${state.year.rangeStart}-${state.year.rangeEnd}`
            break
        }
        return years
      }),
      cron: computed(() => {
        return `${state.secondsText || '*'} ${state.minutesText || '*'} ${state.hoursText || '*'} ${state.daysText || '*'} ${state.monthsText || '*'} ${state.weeksText || '?'} ${
          state.yearsText || '*'
        }`
      }),
      // 计算两个周期值
      cycleTotal: computed(() => {
        state.week.cycle01 = checkNumber(state.week.cycle01, 1, 7)
        state.week.cycle02 = checkNumber(state.week.cycle02, 1, 7)
        console.log('check', state.week)
        return `${state.week.cycle01}-${state.week.cycle02}`
      }),
      // 计算平均用到的值
      averageTotal: computed(() => {
        state.week.average01 = checkNumber(state.week.average01, 1, 4)
        state.week.average02 = checkNumber(state.week.average02, 1, 7)
        return `${state.week.average02}#${state.week.average01}`
      }),
    })
    const handleChange = () => {
      if (typeof state.cron !== 'string')
        return false
      emit('change', state.cron)
    }
    // 清空
    const clearCron = () => {
      state.second.cronEvery = '1'
      state.minute.cronEvery = '1'
      state.hour.cronEvery = '1'
      state.day.cronEvery = '1'
      state.month.cronEvery = '1'
      state.week.cronEvery = '1'
      state.year.cronEvery = '1'
    }
    // 表单选项的子组件校验数字格式（通过-props传递）
    const checkNumber = (value, minLimit, maxLimit) => {
      // 检查必须为整数
      value = Math.floor(value)
      if (value < minLimit)
        value = minLimit
      else if (value > maxLimit)
        value = maxLimit

      return value
    }
    const tabActive = ref(1)
    const currYear = new Date().getFullYear() - 1
    const onHandleTab = (index) => {
      tabActive.value = index
    }
    watch(
      () => state.cron,
      (value) => {
        if (typeof state.cron !== 'string')
          return
        emit('update:value', value)
      },
    )
    return {
      state,
      tabActive,
      currYear,
      weekList,
      onHandleTab,
      handleChange,
      clearCron,
      checkNumber,
    }
  },
})
</script>

<template>
  <div class="v3c">
    <ul class="v3c-tab">
      <li class="v3c-tab-item" :class="{ 'v3c-active': tabActive == 1 }" @click="onHandleTab(1)">
        秒
      </li>
      <li class="v3c-tab-item" :class="{ 'v3c-active': tabActive == 2 }" @click="onHandleTab(2)">
        分
      </li>
      <li class="v3c-tab-item" :class="{ 'v3c-active': tabActive == 3 }" @click="onHandleTab(3)">
        时
      </li>
      <li class="v3c-tab-item" :class="{ 'v3c-active': tabActive == 4 }" @click="onHandleTab(4)">
        日
      </li>
      <li class="v3c-tab-item" :class="{ 'v3c-active': tabActive == 5 }" @click="onHandleTab(5)">
        月
      </li>
      <li class="v3c-tab-item" :class="{ 'v3c-active': tabActive == 7 }" @click="onHandleTab(7)">
        周
      </li>
      <li class="v3c-tab-item" :class="{ 'v3c-active': tabActive == 6 }" @click="onHandleTab(6)">
        年
      </li>
    </ul>

    <!-- 秒 -->
    <div v-show="tabActive == 1" class="v3c-content">
      <!-- 每一秒 -->
      <el-form-item>
        <el-radio v-model="state.second.cronEvery" label="1">
          每一秒钟
        </el-radio>
      </el-form-item>
      <!-- 每隔多久 -->
      <el-form-item>
        <el-radio v-model="state.second.cronEvery" :label="2">
          从
          <el-input-number v-model="state.second.incrementStart" type="number" :min="0" :max="59" />
          秒开始，每 <el-input-number v-model="state.second.incrementIncrement" type="number" :min="1" :max="60" />
          秒执行一次
        </el-radio>
      </el-form-item>
      <!-- 周期 -->
      <el-form-item>
        <el-radio v-model="state.second.cronEvery" :label="4">
          周期从
          <el-input-number v-model="state.second.rangeStart" type="number" :min="1" :max="60" />
          -
          <el-input-number v-model="state.second.rangeEnd" type="number" :min="0" :max="59" />
          秒
        </el-radio>
      </el-form-item>
      <!-- 具体秒数 -->
      <el-form-item>
        <el-radio v-model="state.second.cronEvery" :label="3">
          指定
          <el-select v-model="state.second.specificSpecific" clearable placeholder="可多选" multiple style="width: 100%">
            <el-option v-for="item in 60" :key="item" :value="item - 1">
              {{ item - 1 }}
            </el-option>
          </el-select>
        </el-radio>
      </el-form-item>
    </div>
    <!-- 分钟 -->
    <div v-show="tabActive == 2" class="v3c-content">
      <!-- 每一分 -->
      <el-form-item>
        <el-radio v-model="state.minute.cronEvery" label="1">
          分钟 允许的通配符[, - * /]
        </el-radio>
      </el-form-item>

      <!-- 每隔多久 -->
      <el-form-item>
        <el-radio v-model="state.minute.cronEvery" :label="2">
          从
          <el-input-number v-model="state.minute.incrementStart" type="number" :min="0" :max="59" />
          分开始 每
          <el-input-number v-model="state.minute.incrementIncrement" type="number" :min="1" :max="60" />
          分执行一次
        </el-radio>
      </el-form-item>
      <!-- 具体分钟数 -->
      <el-form-item>
        <el-radio v-model="state.minute.cronEvery" :label="4">
          周期从
          <el-input-number v-model="state.minute.rangeStart" type="number" :min="1" :max="60" />
          -
          <el-input-number v-model="state.minute.rangeEnd" type="number" :min="0" :max="59" />
          分
        </el-radio>
      </el-form-item>
      <!-- 具体分钟数 -->
      <el-form-item>
        <el-radio v-model="state.minute.cronEvery" :label="3">
          指定
          <el-select v-model="state.minute.specificSpecific" multiple>
            <el-option v-for="(item, index) in 60" :key="index" :value="index">
              {{ index }}
            </el-option>
          </el-select>
        </el-radio>
      </el-form-item>
    </div>
    <!-- 3.小时 -->
    <div v-show="tabActive == 3" class="v3c-content">
      <!-- 每一小时 -->
      <el-form-item>
        <el-radio v-model="state.hour.cronEvery" label="1">
          小时 允许的通配符[, - * /]
        </el-radio>
      </el-form-item>

      <!-- 每隔多久小时 -->
      <el-form-item>
        <el-radio v-model="state.hour.cronEvery" :label="2">
          从
          <el-input-number v-model="state.hour.incrementStart" type="number" :min="0" :max="59" />
          小时开始，每隔
          <el-input-number v-model="state.hour.incrementIncrement" type="number" :min="1" :max="60" />
          小时执行一次
        </el-radio>
      </el-form-item>

      <!-- 周期 -->
      <el-form-item>
        <el-radio v-model="state.hour.cronEvery" :label="4">
          周期从
          <el-input-number v-model="state.hour.rangeStart" type="number" :min="1" :max="60" />
          -
          <el-input-number v-model="state.hour.rangeEnd" type="number" :min="0" :max="59" />
          小时
        </el-radio>
      </el-form-item>
      <!-- 具体小时数 -->
      <el-form-item>
        <el-radio v-model="state.hour.cronEvery" :label="3">
          指定
          <el-select v-model="state.hour.specificSpecific" multiple>
            <el-option v-for="(item, index) in 60" :key="index" :value="index">
              {{ index }}
            </el-option>
          </el-select>
        </el-radio>
      </el-form-item>
    </div>
    <!-- 4.日 -->
    <div v-show="tabActive == 4" class="v3c-content">
      <!-- 1 -->
      <el-form-item>
        <el-radio v-model="state.day.cronEvery" label="1">
          每天
        </el-radio>
      </el-form-item>
      <!-- 2 -->
      <el-form-item>
        <el-radio v-model="state.day.cronEvery" label="2">
          不指定
        </el-radio>
      </el-form-item>
      <!-- 3 -->
      <el-form-item>
        <el-radio v-model="state.day.cronEvery" label="3">
          周期从
          <el-input-number v-model="state.day.cycle01" :min="1" :max="30" /> -
          <el-input-number v-model="state.day.cycle02" :min="state.day.cycle01 ? state.day.cycle01 + 1 : 2" :max="31" /> 日
        </el-radio>
      </el-form-item>
      <!-- 4 -->
      <el-form-item>
        <el-radio v-model="state.day.cronEvery" label="4">
          从
          <el-input-number v-model="state.day.incrementStart" :min="1" :max="30" /> 号开始，每
          <el-input-number v-model="state.day.incrementIncrement" :min="1" :max="31 - state.day.incrementStart || 1" /> 日执行一次
        </el-radio>
      </el-form-item>
      <!-- 5 -->
      <el-form-item>
        <el-radio v-model="state.day.cronEvery" label="5">
          最近的工作日(周一至周五)至本月
          <el-input-number v-model="state.day.cronDaysNearestWeekday" :min="1" :max="31" />
          日
        </el-radio>
      </el-form-item>
      <!-- 6 -->
      <el-form-item>
        <el-radio v-model="state.day.cronEvery" label="6">
          在这个月的最后一天
        </el-radio>
      </el-form-item>
      <!-- 7 -->
      <el-form-item>
        <el-radio v-model="state.day.cronEvery" label="7">
          指定
          <el-select v-model="state.day.specificSpecific" multiple>
            <el-option v-for="(val, index) in 31" :key="index" :value="val">
              {{ val }}
            </el-option>
          </el-select>
        </el-radio>
      </el-form-item>
    </div>
    <!-- 7.周 -->
    <div v-show="tabActive == 7" class="v3c-content">
      <!-- 1 -->
      <el-form-item>
        <el-radio v-model="state.week.cronEvery" label="1">
          每周
        </el-radio>
      </el-form-item>
      <!-- 2 -->
      <el-form-item>
        <el-radio v-model="state.week.cronEvery" label="2">
          不指定
        </el-radio>
      </el-form-item>

      <!-- 3 -->
      <el-form-item>
        <el-radio v-model="state.week.cronEvery" label="3">
          周期从星期
          <el-select v-model="state.week.cycle01" clearable style="width: 100px">
            <el-option v-for="(item, index) of weekList" :key="index" :label="item.value" :value="item.key" :disabled="item.key === 1">
              {{ item.value }}
            </el-option>
          </el-select>
          -
          <el-select v-model="state.week.cycle02" clearable style="width: 100px">
            <el-option v-for="(item, index) of weekList" :key="index" :label="item.value" :value="item.key" :disabled="item.key < state.week.cycle01 && item.key !== 1">
              {{ item.value }}
            </el-option>
          </el-select>
        </el-radio>
      </el-form-item>

      <!-- 4 -->
      <el-form-item>
        <el-radio v-model="state.week.cronEvery" label="4">
          第
          <el-input-number v-model="state.week.average01" :min="1" :max="4" /> 周的星期
          <el-select v-model="state.week.average02" clearable style="width: 100px">
            <el-option v-for="(item, index) of weekList" :key="index" :label="item.value" :value="item.key">
              {{ item.value }}
            </el-option>
          </el-select>
        </el-radio>
      </el-form-item>
      <!-- 5 -->
      <el-form-item>
        <el-radio v-model="state.week.cronEvery" label="5">
          本月最后一个星期
          <el-select v-model="state.week.weekday" clearable style="width: 100px">
            <el-option v-for="(item, index) of weekList" :key="index" :label="item.value" :value="item.key">
              {{ item.value }}
            </el-option>
          </el-select>
        </el-radio>
      </el-form-item>
      <!-- 6 -->
      <el-form-item>
        <el-radio v-model="state.week.cronEvery" label="6">
          指定
          <el-select v-model="state.week.specificSpecific" clearable placeholder="可多选" multiple style="width: 100%">
            <el-option v-for="(item, index) of weekList" :key="index" :label="item.value" :value="String(item.key)">
              {{ item.value }}
            </el-option>
          </el-select>
        </el-radio>
      </el-form-item>
    </div>
    <!-- 5.月 -->
    <div v-show="tabActive == 5" class="v3c-content">
      <!-- 1 -->
      <div>
        <label for="month1">
          <input id="month1" v-model="state.month.cronEvery" type="radio" value="1">
          月 允许的通配符[, - * /]
        </label>
      </div>
      <!-- 2 -->
      <div class="mt-20">
        <label for="month2">
          <input id="month2" v-model="state.month.cronEvery" type="radio" value="2">
          从
          <el-input-number v-model="state.month.incrementStart" type="number" :min="1" :max="11" />
          月开始，每隔
          <el-input-number v-model="state.month.incrementIncrement" type="number" :min="1" :max="12 - state.month.incrementStart || 0" />
          月月执行一次
        </label>
      </div>
      <!-- 3 -->
      <div class="mt-20">
        <label for="month3">
          <input id="month3" v-model="state.month.cronEvery" type="radio" value="3">
          指定
          <el-select v-model="state.month.specificSpecific" multiple>
            <el-option v-for="(val, index) in 12" :key="index" :value="val">
              {{ val }}
            </el-option>
          </el-select>
        </label>
      </div>
      <!-- 4 -->
      <div class="mt-20">
        <label for="month4">
          <input id="month4" v-model="state.month.cronEvery" type="radio" value="4">
          周期从
          <el-input-number v-model="state.month.rangeStart" type="number" :min="1" :max="11" />
          -
          <el-input-number v-model="state.month.rangeEnd" type="number" :min="state.month.rangeStart ? state.month.rangeStart + 1 : 2" :max="12" />
          月
        </label>
      </div>
    </div>
    <!-- 6.年 -->
    <div v-show="tabActive == 6" class="v3c-content">
      <!-- 1 -->
      <div>
        <label for="year1">
          <input id="year1" v-model="state.year.cronEvery" type="radio" value="1">
          每年
        </label>
      </div>
      <!-- 2 -->
      <div class="mt-20">
        <label for="year2">
          <input id="year2" v-model="state.year.cronEvery" type="radio" value="2">
          从
          <el-input-number v-model="state.year.incrementStart" type="number" :min="currYear" :max="currYear + 10" />
          年开始，每
          <el-input-number v-model="state.year.incrementIncrement" type="number" :min="1" :max="99" />
          年执行一次
        </label>
      </div>
      <!-- 4 -->
      <div class="mt-20">
        <label for="year3">
          <input id="year3" v-model="state.year.cronEvery" type="radio" value="4">
          周期从
          <el-input-number v-model="state.year.rangeStart" type="number" :min="currYear + 1" :max="currYear + 10" />
          -
          <el-input-number v-model="state.year.rangeEnd" type="number" :min="state.year.rangeStart ? state.year.rangeStart + 1 : currYear + 1" :max="currYear + 10" />
        </label>
      </div>
      <!-- 3 -->
      <div class="mt-20">
        <label for="year3">
          <input id="year3" v-model="state.year.cronEvery" type="radio" value="3">
          指定
          <el-select v-model="state.year.specificSpecific" multiple>
            <el-option v-for="(val, index) in 100" :key="index" :value="currYear + val">
              {{ currYear + val }}
            </el-option>
          </el-select>
        </label>
      </div>
    </div>
    <!-- 结果 -->
    <div class="v3c-footer">
      <fieldset>
        <legend>Cron表达式</legend>
        <span class="cron">{{ state.cron }}</span>
      </fieldset>
    </div>

    <div class="v3c-footer">
      <div>
        <el-button size="small" type="primary" @click="handleChange">
          保存
        </el-button>
        <el-button size="small" type="warning" @click="clearCron">
          重置
        </el-button>
      </div>
    </div>
  </div>
</template>

<style lang="css" scoped>
.v3c {
  width: auto;
  border: 1px solid #f5f7fa;
}
.v3c-tab {
  padding: 0;
  list-style: none;
  margin: 0;
  /* background-color: #f5f7fa; */
  display: flex;
}

.v3c-tab-item {
  flex: 1;
  text-align: center;
  cursor: pointer;
  padding: 10px;
}

.v3c-tab-item.v3c-active {
  background-color: #5b8ff9;
  color: #ffffff;
}

.v3c-lang-btn {
  background-color: #61ddaa;
  color: #ffffff;
  /* border-radius: 10px; */
}

.v3c-content {
  padding: 20px;
  max-height: v-bind(maxHeight);
  overflow: hidden;
  overflow-y: auto;
}

.p-20 {
  padding: 20px;
}

.v3c-footer {
  /* background-color: #f5f7fa; */
  /* padding-top: 10px; */
  padding-bottom: 10px;
  /* display: flex; */
  text-align: center;
}

.mt-20 {
  margin-top: 20px;
}

.v3c input[type='text'] {
  width: 80px;
}

.v3c input[type='number'] {
  width: 80px;
  height: 28px;
  border: 1px solid #d9d9d9;
}

.v3c select {
  width: 80px;
  height: 32px;
  border: 1px solid #d9d9d9;
}

.v3c select[multiple] {
  width: 80px;
  height: 100px;
  border: 1px solid #d9d9d9;
}

.btn-ok {
  line-height: 1.5715;
  position: relative;
  display: inline-block;
  font-weight: 400;
  white-space: nowrap;
  text-align: center;
  background-image: none;
  border: 1px solid transparent;
  box-shadow: 0 2px #00000004;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  touch-action: manipulation;
  height: 32px;
  padding: 4px 15px;
  font-size: 14px;
  border-radius: 2px;

  color: #fff;
  background: #5b8ff9;
  border-color: #5b8ff9;
  text-shadow: 0 -1px 0 rgb(0 0 0 / 12%);
  box-shadow: 0 2px #0000000b;
}

.btn-close {
  line-height: 1.5715;
  position: relative;
  display: inline-block;
  font-weight: 400;
  white-space: nowrap;
  text-align: center;
  background-image: none;
  border: 1px solid transparent;
  box-shadow: 0 2px #00000004;
  cursor: pointer;
  transition: all 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
  -webkit-user-select: none;
  -moz-user-select: none;
  -ms-user-select: none;
  user-select: none;
  touch-action: manipulation;
  height: 32px;
  padding: 4px 15px;
  font-size: 14px;
  border-radius: 2px;

  /*color: #fff;
   background: #61ddaa;
  border-color: #61ddaa; */
  text-shadow: 0 -1px 0 rgb(0 0 0 / 12%);
  box-shadow: 0 2px #0000000b;
}

.cron {
  /* background-color: #61ddaa; */
  padding: 5px;
  padding-left: 10px;
  padding-right: 10px;
  /* color: #ffffff; */
}
</style>

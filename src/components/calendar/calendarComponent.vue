<template>
  <div class="calendar-month">
    <div class="calendar-month-header">
      <header-calendar :started-date.sync="currSelect"/>
    </div>
    <days-of-week/>
    <ol class="days-grid">
      <one-day-cell v-for="(value,index) in showDays" :key="index" :numberDay="value.numberDay"
                    :disable-day="value.disableDay" :holiday-day="value.holidayDay" :current="value.currentDay"
                    :event-day="value.eventList"/>
    </ol>
  </div>
</template>

<script>
import DaysOfWeek from '@/components/calendar/daysOfWeek';
import oneDayCell from '@/components/calendar/oneDayCell';
import HeaderCalendar from '@/components/calendar/headerCalendar';
import Vue from 'vue';

export default {
  name: 'calendar-component',
  components: {HeaderCalendar, DaysOfWeek, oneDayCell},
  props: {
    eventList: {
      type: Array,
      validator(values) {
        const requiredArrProps = ['id', 'name', 'date', 'type'];
        values.map(o => {
          const keys = Object.keys(o);
          const requiredExist = keys.filter(i => requiredArrProps.includes(i));
          if (requiredExist.length !== requiredArrProps.length) {
            const requiredNotExist = requiredArrProps.filter(i => !keys.includes(i));
            //TODO можно заменить на ошибку если нужна жесткая валидация
            console.warn(
                `Не переданы обязательные параметры: ${requiredNotExist.join(
                    ',')}.Список обязательных параметров: ${requiredArrProps.join(
                    ',')}. Строка с ошибкой: ${JSON.stringify(o)}`);
          }
        });
        return true;
      },
    },
  },
  data() {
    return {
      currSelect: this.moment().format('DD.MM.YYYY'),
      nowDate: this.moment(),
    };
  },

  computed: {
    currentSelected: {
      get() {
        return this.moment(this.currSelect, 'DD.MM.YYYY');
      },
      set(value) {
        Vue.set(this, 'currSelect', value.format('DD.MM.YYYY')); //нужно  потому что реактивность иначе отказывается работать, из-за прикольных объектов.
      },
    },
    nowDateYear() {
      return this.nowDate.format('YYYY');
    },
    startedWeekCurrentMonthDay() {
      return this.moment(this.currentSelected).startOf('month').weekday(0);
    },
    lastDayCurrentMonthDay() {
      return this.moment(this.currentSelected).endOf('month').weekday(6);
    },
    daysInCurrentMonth() {
      return this.moment(this.currentSelected).daysInMonth();
    },
    prevMonthShowDays() {
      const dayInMonth = this.startedWeekCurrentMonthDay.daysInMonth();
      const numberStartedWeekDayInMonth = this.startedWeekCurrentMonthDay.date();
      return numberStartedWeekDayInMonth === 1 ? [] : this.createDaysArray(dayInMonth - numberStartedWeekDayInMonth + 1,
          this.startedWeekCurrentMonthDay);
    },
    nextMonthShowDays() {
      const lastDayMonth = this.lastDayCurrentMonthDay.format('M');
      const currentMonth = this.currentSelected.format('M');
      const numberEndedWeekDayInMonth = this.lastDayCurrentMonthDay.date();
      return lastDayMonth === currentMonth ? [] : this.createDaysArray(numberEndedWeekDayInMonth,
          this.moment(this.lastDayCurrentMonthDay).startOf('month'));
    },
    currentMonthDaysArray() {
      return this.createDaysArray(this.daysInCurrentMonth, this.moment(this.currentSelected).startOf('month'));
    },
    showDays() {
      return [...this.prevMonthShowDays, ...this.currentMonthDaysArray, ...this.nextMonthShowDays];
    },
  },
  methods: {
    createDaysArray(length, startedDate) {
      return Array.from({length: length}, (x, i) => {
        const day = this.moment(startedDate).add(i, 'days');
        return {
          numberDay: day.format('DD'),
          disableDay: day.isBefore(this.nowDate, 'day'),
          currentDay: this.nowDate.isSame(day, 'day'),
          holidayDay: [5, 6].includes(parseInt(day.weekday())), //5 - суббота  6 - вс чекаем выходные.
          eventList: this.eventList.filter(i => this.moment(i.date, 'DD.MM.YYYY').isSame(day)),
        };

      });
    },
  },
};
</script>

<style scoped>
.calendar-month {
  position: relative;
  background-color: var(--grey-200);
  border: solid 1px var(--grey-300);
}

.days-grid {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.day-of-week > * {
  text-align: right;
  padding-right: 5px;
}

.days-grid {
  height: 100%;
  position: relative;
  grid-column-gap: var(--grid-gap);
  grid-row-gap: var(--grid-gap);
  border-top: solid 1px var(--grey-200);
}
</style>
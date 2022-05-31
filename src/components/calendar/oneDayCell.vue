<template>
  <li
      class="calendar-day"
      :class="{
         'calendar-day-today-block' : current,
      }"
  >
    <div>
      <div class="calendar-day-container-number">
         <span class="calendar-day-number"
               :class="{
          'calendar-day--disable' :disableDay,
          'calendar-day--holiday' : holidayDay,
          'calendar-day--today' : current,
          }">{{ numberDay }}</span>
      </div>
      <event-list v-if="eventDay.length" :events="eventDay"/>
    </div>
  </li>
</template>

<script>
import EventList from '@/components/calendar/events/eventList';

export default {
  name: 'oneDayCell',
  components: {EventList},
  props: ['numberDay', 'disableDay', 'holidayDay', 'current', 'eventDay'],
};
</script>

<style scoped>
.calendar-day {
  position: relative;
  min-height: 100px;
  font-size: 16px;
  background-color: #fff;
  color: var(--grey-800);
  padding: 5px;
}

.calendar-day > div > span {
  float: right;
  right: 2px;
  width: var(--day-label-size);
  height: var(--day-label-size);
}

.calendar-day--disable {
  color: var(--grey-300);
}

.calendar-day--holiday {
  color: var(--holiday);
}

.calendar-day-today-block {
  padding-top: 4px;
  border: 1px solid var(--grey-800);
}

.calendar-day--today {
  color: var(--current-day);
}

.calendar-day-number {
  z-index: 3;
  background: white;
}
.calendar-day-container-number {
  text-align: right;
}

</style>
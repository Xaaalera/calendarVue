<template>
  <div id="app">
    <calendar-component :event-list="eventList"/>
  </div>
</template>

<script>

import CalendarComponent from '@/components/calendar/calendarComponent';

export default {
  name: 'App',
  components: {
    CalendarComponent,
  },
  data() {
    return {
      eventList: [
        {id: 0, name: 'Сходить к стоматологу', date: '23.05.2022 16:00', type: 3},
        {id: 7, name: 'Пройти курсы обучения', date: '18.05.2022 14:00', type: 2},
        {id: 1, name: 'Пройти курсы обучения', date: '18.05.2022 12:00', type: 2},
        {id: 2, name: 'Забрать вещи', date: '19.05.200 13:00', type: 1},
        {id: 3, name: 'Уроки по гитаре', date: '19.05.2022 14:00', type: 1},
        {id: 6, name: 'Уроки по гитаре', date: '19.05.2022 17:00', type: 2},
        {id: 4, name: 'Приезд свекрови', date: '23.05.2022 18:00', type: 3},
        {id: 5, name: 'Приезд свекрови', date: '24.05.2022 19:00', type: 3},

      ],
    };
  },
};
</script>
<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

body {
  font-family: sans-serif;
  font-weight: 100;
  --grey-100: #e4e9f0;
  --grey-200: #cfd7e3;
  --grey-300: #b5c0cd;
  --grey-800: #3e4e63;
  --grid-gap: 1px;
  --day-label-size: 20px;
  --holiday: purple;
  --current-day: green;
  --navigate-title: blue;
}

ol,
li {
  padding: 0;
  margin: 0;
  list-style: none;
}

.calendar-month-header {
  display: flex;
  justify-content: space-between;
  background-color: #fff;
  padding: 10px;
}
</style>

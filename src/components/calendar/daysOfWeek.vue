<template>

  <ol class="day-of-week">
    <li v-for="weekday in moment.weekdaysShort(true)" :key="weekday">{{ weekday }}</li>
  </ol>
</template>

<script>

export default {
  name: 'DaysOfWeek',
};
</script>

<style scoped>
.day-of-week {
  display: grid;
  grid-template-columns: repeat(7, 1fr);
}

.day-of-week > * {
  text-align: right;
  padding-right: 5px;
}
</style>

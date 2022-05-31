<template>
  <div class="calendar-date-indicator">
    <span class="navigate" @click="downMonth">&lt;</span>
    <span class="title">{{ selectedMonth }} {{ showYear ? selectedYear : '' }}</span>
    <span class="navigate" @click="upMonth">&gt;</span>
  </div>
</template>

<script>
export default {
  name: 'headerCalendar',
  props: {
    startedDate: {
      type: String,
      required: true,
    },
  },
  computed: {
    selectedDate: {
      get() {

        return this.moment(this.startedDate, 'DD.MM.YYYY');
      },
      set(value) {

        this.$emit('update:startedDate', value.format('DD.MM.YYYY'));
      },
    },
    selectedMonth() {
      return this.selectedDate.format('MMMM');
    },
    selectedYear() {
      return this.selectedDate.format('YYYY');
    },
    showYear() {
      return !this.selectedDate.isSame(this.moment(), 'year');
    },
  },
  methods: {
    downMonth() {
      this.selectedDate = this.selectedDate.startOf('month').subtract(1, 'month');

    },
    upMonth() {
      this.selectedDate = this.selectedDate.startOf('month').add(1, 'month');
    },
  },
};
</script>

<style scoped>
.calendar-date-indicator {
  font-size: 24px;
  font-weight: 600;

}

.calendar-date-indicator > .title {
  color: var(--navigate-title);
  text-transform: capitalize;
}

.calendar-date-indicator > .navigate {
  cursor: pointer;
  padding: 0 5px;
}
</style>


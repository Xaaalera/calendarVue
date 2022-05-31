<template>
  <li class="calendar-day-event-item">
    &nbsp;
    <div class="calendar-day-event-item--body event-card"
         :class="currentClass"
    >
      <span>{{ hourEvent }} {{ title }}</span>
    </div>
  </li>
</template>

<script>
export default {
  name: 'eventItem',
  props: {
    id: {
      type: Number,
      required: true,
    },
    title: {
      type: String,
      required: true,
    },
    dataEvent: {
      type: String,
    },
    typeEvent: {
      type: Number,
      default: 1,
      validator(value) {
        const listExistType = [1, 2, 3];
        const confirmed = listExistType.includes(value);
        if (confirmed) {
          return true;
        }
        //TODO выкидывать ошибку если нужно будет
        console.warn(
            `Переданный тип не распознан. Вы передаете ${value}. Допустимые варианты - ${listExistType.join(',')} `);
        return 1;
      },
    },
  },
  computed: {
    hourEvent() {

      return this.moment(this.dataEvent,'DD.MM.YYYY HH:mm').format('HH:mm');
    },
    currentClass() {
      switch (this.typeEvent) {
        case 1:
          return 'event-card--yellow';
        case 2:
          return 'event-card--green';
        case 3:
          return 'event-card--red';
        default:
          return 'event-card--yellow';
      }
    },
  },

};
</script>

<style lang="scss" scoped>
.calendar-day-event-item {
  white-space: nowrap;
  overflow: hidden;
  position: relative;
  cursor: pointer;
  margin: 2px 0;
}

.calendar-day-event-item:hover {
  overflow: visible !important;

  .calendar-day-event-item--body {
    overflow: visible;
    z-index: 99999;
  }
}

.calendar-day-event-item--body {
  position: absolute;
  top: 0;
  left: 0;
  z-index: 2;
  width: 100%;
  overflow: hidden;
  text-overflow: ellipsis;
}

.event-card {
  border-radius: 3px;
  span {
    border-radius: 3px;
    padding: 0 2px;
  }
}
.event-card--yellow {
  background: #f1f1c1;
  span {
    background: #f1f1c1;
    color: #787806;
  }
}

.event-card--green {
  background: #9fd78f;
  span {
    background: #9fd78f;
    color: green;
  }
}

.event-card--red {
  background: #e2a6a6;
  span {
    background: #e2a6a6;
    color: red;
  }
}
</style>
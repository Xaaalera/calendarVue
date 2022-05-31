<template>
    <ol class="event-list">
      <event-item v-for="(value) in sortedEventDay" :key="value.id" :id="value.id" :title="value.name" :data-event="value.date" :type-event="value.type" />
    </ol>
</template>

<script>
import EventItem from '@/components/calendar/events/eventItem';
export default {
  name: 'eventList',
  components: {EventItem},
  props:{
    events:{
      type:Array,
    },
  },
  computed:{
    sortedEventDay(){
      return [...this.events].sort((a,b) => {
        const firstDate =  this.moment(a.date, 'DD.MM.YYYY HH:mm');
        const secondDate = this.moment(b.date, 'DD.MM.YYYY HH:mm');
        return firstDate - secondDate;
      })
    }
  }
};
</script>

<style scoped>
.event-list{
  text-align: left;
}
</style>
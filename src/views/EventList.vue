<template>
  <h1>Events for Good</h1>
  <div class="events">
    <EventCard v-for="event in events" :key="event.id" :event="event" />
  </div>
  <h1>You are on page {{ page }}</h1>
  <h1>Tested {{ tested }} times</h1>
  <h1>Limit {{ limit }}</h1>
</template>

<script>
import EventCard from '@/components/EventCard.vue'
import EventService from '@/services/EventService.js'

export default {
  name: 'EventList',
  components: {
    EventCard
  },
  props: ['testedTimes'],
  data() {
    return {
      events: null
    }
  },
  created() {
    EventService.getEvents()
      .then(response => {
        this.events = response.data
      })
      .catch(error => {
        console.log(error)
      })
  },
  computed: {
    page() {
      return parseInt(this.$route.query.page) || 1
    },
    tested() {
      return this.testedTimes ? this.testedTimes : 0
    },
    limit() {
      return parseInt(this.$route.params.limit) || 0
    }
  }
}
</script>

<style scoped>
.events {
  display: flex;
  flex-direction: column;
  align-items: center;
}
</style>

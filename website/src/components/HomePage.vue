<template>
  <div class="container-lg p-4">
    <div class="Subhead">
      <h2 class="Subhead-heading">Upcoming events</h2>
      <div class="Subhead-actions">
        <router-link
          to="/list"
          class="btn btn-sm btn-secondary">Event list</router-link>
      </div>
    </div>
    <p>
      These are the upcoming tech events.
      You can subscribe to the calendar via <a href="https://calendar.google.com/calendar/embed?src=j5i0o6v2ihfboe19upl9lhonbci6ankr%40import.calendar.google.com&ctz=Asia%2FBangkok">Google Calendar</a>.
    </p>
    <div
      v-if="!!error"
      class="flash flash-error">
      <strong>Cannot load data.</strong> {{ error.toString() }}
    </div>
    <div
      v-if="loading"
      class="Box mt-3">
      <ul>
        <li class="Box-row text-center p-4">
          <spinner/>
        </li>
      </ul>
    </div>
    <div
      v-if="topUpcomingEvents.length > 0"
      class="Box mt-3">
      <ul>
        <li
          v-for="event in topUpcomingEvents"
          :key="event.id"
          class="Box-row">
          <event :event="event"/>
        </li>
      </ul>
      <div
        v-if="upcomingEvents.length > topUpcomingEvents.length"
        class="Box-footer Box-row--gray text-center">
        <router-link to="/list">See more events &rarr;</router-link>
      </div>
    </div>
    <div class="mt-3">
      <p>Missing events or details incorrect? <a href="https://github.com/ThaiProgrammer/tech-events-calendar/tree/master/CONTRIBUTING.md">Please contribute!</a></p>
    </div>

    <div class="Subhead Subhead--spacious">
      <h2 class="Subhead-heading">2019 calendar</h2>
    </div>
    <div class="year-calendar">
      <!--
        @todo #101 Replace this image list with an actual SVG calendar.
         Tiles with no events are not clickable.
         Tiles with events should take you to event list page, scrolling to the first event on that day,
         possibly highlighting the events on that day.
      -->
      <router-link to="/list">
        <img
          src="/generated/calendar-images/2019-01.svg"
          alt="January 2019"
          width="200"
          height="185" >
        <img
          src="/generated/calendar-images/2019-02.svg"
          alt="February 2019"
          width="200"
          height="185" >
        <img
          src="/generated/calendar-images/2019-03.svg"
          alt="March 2019"
          width="200"
          height="185" >
        <img
          src="/generated/calendar-images/2019-04.svg"
          alt="April 2019"
          width="200"
          height="185" >
        <img
          src="/generated/calendar-images/2019-05.svg"
          alt="May 2019"
          width="200"
          height="185" >
        <img
          src="/generated/calendar-images/2019-06.svg"
          alt="June 2019"
          width="200"
          height="185" >
        <img
          src="/generated/calendar-images/2019-07.svg"
          alt="July 2019"
          width="200"
          height="185" >
        <img
          src="/generated/calendar-images/2019-08.svg"
          alt="August 2019"
          width="200"
          height="185" >
        <img
          src="/generated/calendar-images/2019-09.svg"
          alt="September 2019"
          width="200"
          height="185" >
        <img
          src="/generated/calendar-images/2019-10.svg"
          alt="October 2019"
          width="200"
          height="185" >
        <img
          src="/generated/calendar-images/2019-11.svg"
          alt="November 2019"
          width="200"
          height="185" >
        <img
          src="/generated/calendar-images/2019-12.svg"
          alt="December 2019"
          width="200"
          height="185" >
      </router-link>
    </div>
  </div>
</template>

<script>
import { mapState } from 'vuex'
import Event from './Event'
import Spinner from './Spinner'

export default {
  components: { Event, Spinner },
  computed: {
    ...mapState(['loading', 'error', 'events']),
    upcomingEvents() {
      const now = new Date()
      now.setHours(0, 0, 0, 0)
      return this.events.filter(event => {
        const date = new Date(
          event.start.year,
          event.start.month - 1,
          event.start.date
        )
        return date >= now
      })
    },
    topUpcomingEvents() {
      return this.upcomingEvents.slice(0, 5)
    }
  }
}
</script>

<style scoped>
.year-calendar {
  max-width: 888px;
  margin: 0 auto;
  text-align: center;
}
</style>

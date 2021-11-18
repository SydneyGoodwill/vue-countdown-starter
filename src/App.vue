<template>
<button @click="events.push({
    id: 7,
    name: 'new event',
    details: 'new details',
    date: '2020-09-25',
    background: '#F34949',
  })">dark mode</button>
{{ events }}
<ul>
  <li v-for="event in events" :key="event.id">
    <Event :event="event" :daysLeft="daysLeft(event)" />
  </li>
</ul>
</template>

<script>
const eventData = [
  {
    id: 1,
    name: "Graduation",
    details: "wooohoo!!!",
    date: "2020-09-25",
    background: "#F34949",
  },
  {
    id: 2,
    name: "Holidays",
    details: "wooohoo!!!",
    date: "2021-12-30",
    background: "#f9f970",
  },
  {
    id: 3,
    name: "Dentist",
    details: "just a cleaning, whew!",
    date: "2021-11-17",
    background: "#7AD3F0",
  },
  {
    id: 4,
    name: "Birthday",
    details: "My birthday party",
    date: "2021-12-31",
    background: "#F07AEC",
  },
  {
    id: 5,
    name: "Christmas",
    details: "ho ho ho",
    date: "2021-12-25",
    background: "#EB9A0F",
  },
  {
    id: 6,
    name: "Conference Talk",
    details: "dont flop",
    date: "2021-11-18",
    background: "#68EE94",
  },
];

import Event from "./components/Event.vue";

export default {
  name: "App",
  components: {
    Event,
  },
  data() {
    return {
      events: eventData,
      darkModeSet: false,
    };
  },
  methods: {
    daysLeft(event) {
      const Time = Date.parse(event.date) - Date.now();
      const Days = Math.ceil(Time / (1000 * 3600 * 24));
      return Days
    },
  },
  watch: {
    events: {
      deep: true,
      immediate: true,
      handler(val) {
        console.log(val);
      }
    },
  },
};
</script>

<style >
#app {
  font-family: Avenir, Arial, Helvetica, sans-serif;
  max-width: 600px;
  margin: 0 auto;
}

ul {
  padding: 0;
}

li {
  list-style: none;
  cursor: pointer;
}
</style>

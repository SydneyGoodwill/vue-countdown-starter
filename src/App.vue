<template>
  <teleport to="#modal">
    <AddUpdateForm
      v-if="showForm"
      @close-form="closeForm()"
      @add-new-event="add($event)"
      @update-event="update($event)"
      :currentEvent="currentEvent"
    />
  </teleport>
  <div class="options">
    <div class="option_buttons">
      <button @click="showPastEvents = !showPastEvents">
        show past events
      </button>
      <button @click="grayModeSet = !grayModeSet">
        {{ grayModeSet ? "&#9788;" : "&#9789;" }}
      </button>
    </div>
    <button class="addNew" @click="showForm = !showForm">&#43;</button>
  </div>
  <ul>
    <li v-for="event in orderEvents" :key="event.id" @click="setForm(event)">
      <Event
        :style="grayModeSet ? grayMode : ''"
        :event="event"
        :daysLeft="daysLeft(event)"
        :showPastEvents="showPastEvents"
        @remove-event="remove($event)"
      />
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
import AddUpdateForm from "./components/AddUpdateForm.vue";

export default {
  name: "App",
  components: {
    Event,
    AddUpdateForm,
  },
  data() {
    return {
      grayModeSet: false,
      grayMode: {
        background: "lightslategray",
        color: "#454444",
      },
      events: eventData,
      showPastEvents: true,
      showForm: false,
      currentEvent: {},
    };
  },
  methods: {
    findEventIndex(id) {
      return this.events.findIndex((el) => el.id === id);
    },
    remove(event) {
      this.events.splice(this.findEventIndex(event.id), 1);
    },
    closeForm() {
      this.showForm = false;
      this.currentEvent = {};
    },
    update(event) {
      this.events[this.findEventIndex(event.id)] = event;
    },
    setForm(event) {
      this.currentEvent = event || {};
      this.showForm = true;
    },
    add(event) {
      event.id = this.events.length + 1;
      this.events.push(event);
      // this.showForm = false;
    },
    daysLeft(event) {
      const Time = Date.parse(event.date) - Date.now();
      const Days = Math.ceil(Time / (1000 * 3600 * 24));
      return Days;
    },
  },
  computed: {
    orderEvents() {
      const eventsToOrder = this.events;
      return eventsToOrder.sort((a, b) => (a.date > b.date ? 1 : -1));
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

.options {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.addNew {
  font-size: 3rem;
  color: rgb(92, 84, 84);
  cursor: pointer;
  background: none;
  border: none;
}

.option_buttons > button {
  padding: 0.5rem 1rem;
  margin-right: 1rem;
  background: none;
  border-radius: 1rem;
  border: 2px lightsteelblue solid;
  font-size: 1.2rem;
  cursor: pointer;
}
</style>

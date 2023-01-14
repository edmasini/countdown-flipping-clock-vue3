<template>
  <div class="clock">
    <Flipper
      :value="daysStr"
      :bgColor="bgColor"
      :fontColor="fontColor"
      :shadowOpacity="shadowOpacity"
      :fontSize="fontSize"
      :aspectRatio="aspectRatio"
      :label="labels.days"
    />
    <Flipper
      :value="hoursStr"
      :bgColor="bgColor"
      :fontColor="fontColor"
      :shadowOpacity="shadowOpacity"
      :fontSize="fontSize"
      :aspectRatio="aspectRatio"
      :label="labels.hours"
    />
    <Flipper
      :value="minutesStr"
      :bgColor="bgColor"
      :digits="2"
      :fontColor="fontColor"
      :shadowOpacity="shadowOpacity"
      :fontSize="fontSize"
      :aspectRatio="aspectRatio"
      :label="labels.minutes"
    />
    <Flipper
      :value="secondsStr"
      :bgColor="bgColor"
      :digits="2"
      :fontColor="fontColor"
      :shadowOpacity="shadowOpacity"
      :fontSize="fontSize"
      :aspectRatio="aspectRatio"
      :label="labels.seconds"
    />
  </div>
</template>

//
<style lang="scss" scoped>
.clock {
  display: flex;
  flex-direction: row;
  gap: 5px;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>

<script>
import Flipper from "./flipper.vue";

export default {
  name: "Clock",
  components: { Flipper },
  props: {
    targetDate: {
      type: String,
      required: true,
    },
    bgColor: {
      type: String,
      default: "black",
    },
    fontColor: {
      type: String,
      default: "white",
    },
    shadowOpacity: {
      type: Number,
      default: 0.5,
    },
    fontSize: {
      type: String,
      default: "140px",
    },
    aspectRatio: {
      type: Number,
      default: 4,
    },
    labels: {
      default: {
        days: "days",
        hours: "hours",
        minutes: "minutes",
        seconds: "seconds",
      },
    },
  },
  data() {
    return {
      seconds: 0,
      minutes: 0,
      days: 0,
      hours: 0,
    };
  },
  computed: {
    daysStr() {
      return this.days;
    },
    hoursStr() {
      return this.hours;
    },
    minutesStr() {
      return this.minutes;
    },
    secondsStr() {
      return this.seconds;
    },
  },
  methods: {
    updateClock() {
      const now = new Date();
      var futureDate = new Date(this.targetDate);
      const totalSecs = Math.round(
        (futureDate.getTime() - now.getTime()) / 1000
      );
      const rDays = totalSecs / 86400;
      this.days = Math.floor(totalSecs / 86400);
      const rHours = (rDays % 1) * 24;
      this.hours = Math.floor(rHours);
      const rMinutes = (rHours % 1) * 60;
      this.minutes = Math.floor(rMinutes);
      const rSeconds = Math.round((rMinutes % 1) * 60);
      this.seconds = rSeconds > 59 ? 0 : rSeconds;
      // console.log(rDays, rHours, rMinutes, rSeconds);
    },
  },
  mounted() {
    // console.log(this.targetDate);
    this.updateClock();
    setInterval(() => {
      this.updateClock();
    }, 1000);
  },
};
</script>

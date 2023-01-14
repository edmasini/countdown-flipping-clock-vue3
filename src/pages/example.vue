<template>
  <Countdown
    class="clock"
    targetDate="2023-11-20 17:53"
    bgColor="#EADDDF"
    fontColor="#505050"
    :shadowOpacity="0.15"
    :aspectRatio="4"
    fontSize="200px"
    :labels="{
      days: 'days',
      hours: 'hours',
      minutes: 'minutes',
      seconds: 'seconds',
    }"
  />
</template>

<script>
import Countdown from "../components/flipping-clock/clock.vue";

export default {
  name: "example",
  components: { Countdown },
};
</script>

<style scoped>
.clock {
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>

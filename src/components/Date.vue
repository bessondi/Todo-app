<template>
  <div class="date">
    <div class="date__fullDate"><strong>{{fullDate}}</strong></div>
    <div class="date__day">{{day}}</div>
  </div>
</template>

<script>
  const date = new Date()
  const day = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"]
  const month = ["January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December"]
  const fullDate = (`${date.getDate()} ${month[date.getMonth()].substr(0, 3)}. ${date.getFullYear()}`)

  export default {
    data() {
      return {
        date: date.getDate(),
        day: day[date.getDay()],
        month: month[date.getMonth()],
        year: date.getFullYear(),
        fullDate: fullDate
      }
    }
  }
</script>

<style lang="scss" scoped>
  @import '../scss/colors';


  .date {
    color: $accentBlueColor;

    &:hover &__day {
      opacity: 1;
      transition: 0.2s;
    }

    &__day {
      opacity: 0;
      transition: 0.2s;
    }
  }
</style>
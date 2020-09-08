<template>
    <div>{{fullDate}}</div>
</template>

<script>
  const date = new Date()
  const day = [ "Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday" ]
  const month =[ "January", "February", "March", "April", "May", "June", "July", "August", "September", "October", "November", "December" ]
  const fullDate = ( `${date.getDate()} ${month[date.getMonth()].substr(0, 3)}. ${date.getFullYear()}` )

  export default {
    data() {
      return {
        day: date.getDate(),
        month: month[date.getMonth()],
        year: date.getFullYear(),
        fullDate: fullDate
      }
    }
  }
</script>

<style scoped>

</style>
<template>
    <ve-bar-chart :data="chartData" />
</template>

<script>


    module.exports = {
  created () {
    this.chartData = {
      dimensions: {
        name: 'Week',
        data: ['Mon.', 'Tue.', 'Wed.', 'Thu.', 'Fir.', 'Sat.', 'Sun.']
      },
      measures: [{
        name: 'PV',
        data: [256, 767, 1356, 2087, 803, 582, 432]
      }, {
        name: 'UV',
        data: [287, 707, 1756, 1822, 987, 432, 322]
      }]
    }
  }
}
</script>


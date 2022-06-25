<template>
<div>
  <h2>
    {{ title }}
  </h2>
  <div>
    <bar-chart />
  </div>
</div>
</template>

<script>
import BarChart from './bar-chart.vue';

export default {
  name: 'chart',
  components: {
    BarChart
  },
  props: {
    title : {
      type: String,
      required: true,
      default: 'フードファイターチャート',
    },
  },
  mounted: function () {
    // alert('pge');
  }
}
</script>

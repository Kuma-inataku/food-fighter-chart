<template>
<div>
  <h2>
    {{ title }}
  </h2>
</div>
</template>

<script>
export default {
  name: 'chart',
  props: {
    title : {
      type: String,
      required: true,
      default: 'フードファイターチャート',
    },
  },
  mounted: function () {
    alert('pge');
  }
}
</script>

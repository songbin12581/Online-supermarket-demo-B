<template>
  <div class="index">
    <div class="card">
      <card />
      <charts />
    </div>
  </div>
</template>

<script>
import card from '@/components/card.vue';
import charts from '@/components/charts.vue';

export default {
  components: {
    card,
    charts,
  },
};
</script>

<template>
  <div class="home">
    <NavComponent title="Detail Faktur" />
    <DetailGajiComponent /> 
  </div>
</template>

<script>
// @ is an alias to /src
import NavComponent from '@/components/NavComponent.vue'
import DetailGajiComponent from '@/components/DetailGajiComponent.vue'

export default {
  name: 'DetailGajiView',
  components: {
    NavComponent,
    DetailGajiComponent
  }
}
</script>

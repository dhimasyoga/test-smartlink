<template>
  <div class="home">
    <NavComponent title="Faktur Gaji" />
    <FakturGajiComponent />
  </div>
</template>

<script>
// @ is an alias to /src
import FakturGajiComponent from '@/components/FakturGajiComponent.vue'
import NavComponent from '@/components/NavComponent.vue'

export default {
  name: 'MainView',
  components: {
    FakturGajiComponent,
    NavComponent
  }
}
</script>

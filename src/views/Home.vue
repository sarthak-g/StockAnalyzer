<template>
  <div class="home">
    <StocksInfo />
  </div>
</template>

<script>
// @ is an alias to /src
import StocksInfo from '@/components/StocksInfo.vue'

export default {
  name: 'Home',
  components: {
    StocksInfo
  }
}
</script>

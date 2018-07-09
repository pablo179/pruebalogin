<template>
  <div id="log" class="log">
    <LoginComponent />
  </div>
</template>
<style>
  .log{
    width: 100%;
    height: 100%;
  }
</style>

<script>
// @ is an alias to /src
import LoginComponent from '@/components/LoginComponent.vue'

export default {
  name: 'home',
  components: {
    LoginComponent
  }
}

</script>

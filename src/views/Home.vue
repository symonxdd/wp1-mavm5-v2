<template>
  <div>
      <AddAfspraak></AddAfspraak>
  </div>
</template>

<script>
// @ is an alias to /src
import AddAfspraak from '@/components/AddAfspraak.vue'

export default {
  name: 'App',
  components: {
    AddAfspraak
  }    
}
</script>
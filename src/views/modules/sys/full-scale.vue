<template>

</template>

<script>
import mixinViewModule from '@/mixins/view-module'
import AddOrUpdate from './menu-add-or-update'
import axios from "axios";
export default {
  name: "full-scale",
  mixins: [mixinViewModule],
  data () {
    return {
      coupon:[]
    }
  },
  components: {
    AddOrUpdate
  }
}
</script>

<style scoped>

</style>
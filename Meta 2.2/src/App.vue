<template>
  <v-app>
    <v-content>
      <Navbar></Navbar>
      <router-view></router-view>
    </v-content>
  </v-app>
</template>

<script>
import Navbar from "@/components/Navbar.vue";
import {defineComponent} from "vue";

export default defineComponent({
  components: {Navbar}
})

</script>

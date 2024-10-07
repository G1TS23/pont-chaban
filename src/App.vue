<template>
    <div class="main-view">
      <Header />
      <PontChaban />
    </div>
</template>

<script>
import PontChaban from "@/components/PontChaban.vue"
import Header from "@/components/Header.vue"

export default {
  components: {
    PontChaban,
    Header
  },
  data() {
    return {
    }
  }
}
</script>

<style scoped>
.main-view {
  background-color: rgb(32, 36, 41);
  color: white;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  font-family: "Montserrat", sans-serif;
}
</style>
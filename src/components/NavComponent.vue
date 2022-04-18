<template>
  <header>
    <nav class="flex items-center justify-center py-3">
      <img
          @click="reload"
          class="w-8 mx-2"
          src="@/assets/cashflow-icon.svg"
          alt="CashFlow">
      <h1 class="text-main-blue text-3xl font-bold mx-2"><span class="text-main-green">Cash</span>Flow</h1>
    </nav>
  </header>
</template>

<script>
export default {
  methods: {
    reload() {
      location.reload()
    }
  }
}
</script>

<style scoped>

</style>
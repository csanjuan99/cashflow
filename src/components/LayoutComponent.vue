<template>
  <div id="main" class="flex flex-col items-center justify-center h-screen">
    <slot name="header"></slot>
    <slot name="resume"></slot>

    <div class="movements bg-white absolute bottom-0">
      <div class="movements-head cursor-pointer w-screen flex flex-col items-center rounded-t-2xl"
           @click="isOpen=!isOpen">
        <div class="movements-head__grip w-60 h-3 rounded-3xl bg-soft-blue mt-3"/>
        <p class="text-3xl self-start px-3 my-5 text-main-blue font-bold">Historial</p>
      </div>
      <div class="movements-body px-3 bg-white" v-show="isOpen">
        <slot name="movements"></slot>
      </div>
    </div>
  </div>
</template>

<script>
import {ref} from 'vue';

export default {
  setup() {
    const isOpen = ref(false);
    return {
      isOpen,
    }
  },
}

</script>

<style scoped>
.movements-head {
  box-shadow: 0 -10px 16px 0 var(--soft-blue);
}
</style>
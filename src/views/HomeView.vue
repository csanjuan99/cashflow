<template>
  <Suspense>
    <template #fallback>
      <FallbackComponent />
    </template>
    <template #default>
      <HomeComponent />
    </template>
  </Suspense>
</template>

<script>
import FallbackComponent from "@/components/FallbackComponent";
import { defineAsyncComponent} from "vue";

export default {
  name: 'HomeView',
  components: {
    FallbackComponent,
    HomeComponent: defineAsyncComponent(function (){
      return new Promise(function (resolve) {
        setTimeout(function () {
          resolve(import("@/components/HomeComponent"));
        }, 1000);
      });
    })
  },

}
</script>

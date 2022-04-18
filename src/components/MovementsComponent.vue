<template>
  <div class="movements flex flex-col items-center justify-start h-96">
    <div class="movements-content overflow-y-scroll w-full">
      <movement-component
          class="m-5"
          v-for="(movement, i) in movements"
          :key="i"
          :movement="movement"
          @delete-movement="deleteMovement"
      />
    </div>
  </div>
</template>

<script>
import MovementComponent from "@/components/MovementComponent.vue";

export default {
  name: "MovementsComponent",
  props: {
    movements: {
      type: Array,
      required: true
    }
  },
  components: {
    MovementComponent
  },
  methods: {
    deleteMovement(id) {
      this.$emit("delete-movement", id);
    }
  }

}
</script>

<style scoped>
</style>
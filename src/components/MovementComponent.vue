<template>
  <div class="bg-soft-blue py-5 px-3 rounded-lg shadow-sm">
    <div class="grid grid-cols-2">
      <div>
        <h3 class="font-bold">
          {{ movement.name }}
        </h3>
        <p class="font-light">
          {{ movement.description }}
        </p>
        <p class="font-light">
          {{ movement.date }}
        </p>
      </div>
      <div class="flex flex-col items-end">
        <button type="button" @click="deleteMovement">
          <img class="w-5/6" src="@/assets/trash-icon.svg" alt="Close">
        </button>
        <p class="font-bold mt-5" :class="[ movement.type == 1 ? 'text-green-500' : 'text-red-500']">
          <span v-if="movement.type == 1">{{ amount }}</span>
          <span v-else>{{ amount }}</span>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "MovementComponent",
  props: {
    movement: {
      type: Object,
      required: true
    }
  },
  computed: {
    amount() {
      return new Intl.NumberFormat('es-CO', {
        style: 'currency',
        currency: 'COP'
      }).format(this.movement.amount)
    }
  },
  methods: {
    deleteMovement() {
      this.$emit('delete-movement', this.movement.id)
    }
  }
}
</script>

<style scoped>

</style>
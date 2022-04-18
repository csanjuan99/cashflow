<template>
  <div class="flex flex-grow flex-col justify-center items-center resume">
    <p class="resume-label text-lg font-bold">
      {{ labelComputed }}
    </p>
    <p v-if="totalAmount" class="resume-amount text-green-500 text-5xl font-bold"
       :class="[amount < 0 ? 'text-red-500' : '']">
      {{ amountComputed }}
    </p>
    <div class="resume-graph">
      <slot name="graph"></slot>
    </div>
    <div class="resume-add_movement">
      <slot name="add-movement"></slot>
    </div>
  </div>
</template>

<script>
import moment from 'moment';
import 'moment/locale/es'

export default {
  name: "ResumeComponent",
  props: {
    movements: {
      type: Array,
      required: true
    },
    label: {
      type: Array,
      default: () => [],
    },
    totalAmount: {
      type: Number,
    },
    amount: {
      type: Number,
      default: null,
    },
  },
  computed: {
    amountComputed() {
      return new Intl.NumberFormat(
          "es-CO", {
            style: "currency",
            currency: "COP"
          }
      ).format(this.amount !== null ? this.amount : this.totalAmount);
    },
    labelComputed() {
      const label = this.movements.filter(movement => movement.amount === this.amount)
      return label.length === 0 ? "Ahorro Total" : this.formatDate(label[0].date);
    },
  },
  methods: {
    formatDate(date) {
      return moment(date).format("MMMM DD YYYY");
    },
  }
}
</script>

<style scoped>

</style>
<template>
  <layout-component>
    <template #header>
      <nav-component/>
    </template>
    <template #resume>
      <resume-component
          :total-amount="totalAmount"
          :movements="movements"
          :amount="amount">
        <template #graph>
          <div class="p-5">
            <graph-component
                @select="select"
                :amounts="amounts"
            />
          </div>
        </template>
        <template #add-movement>
          <button
              class="my-10"
              @click="open = !open"
              type="button">Agregar movimiento
          </button>
          <Teleport to="body">
            <modal-add-component
                :id="movements.length"
                @add="addMovement"
                @close="open = !open"
                v-if="open"/>
          </Teleport>
        </template>
      </resume-component>
    </template>
    <template #movements>
      <movements-component
          :movements="movements"
          @delete-movement="deleteMovement"
      />
    </template>
  </layout-component>
</template>

<script>
import ModalAddComponent from "@/components/ModalAddComponent";
import LayoutComponent from "@/components/LayoutComponent";
import NavComponent from "@/components/NavComponent";
import ResumeComponent from "@/components/ResumeComponent";
import MovementsComponent from "@/components/MovementsComponent";
import GraphComponent from "@/components/GraphComponent";

export default {
  name: "HomeComponent",
  components: {
    NavComponent,
    LayoutComponent,
    ResumeComponent,
    MovementsComponent,
    ModalAddComponent,
    GraphComponent
  },
  data() {
    return {
      open: false,
      movements: [],
      amount: null,
      label: null,
    }
  },
  mounted() {
    const movements = JSON.parse(localStorage.getItem('movements'));
    if (Array.isArray(movements)) {
      this.movements = movements.map(movement => {
        return {
          ...movement, date: new Date(movement.date)
        }
      })
    }
  },
  computed: {
    amounts() {
      return this.movements.filter(movement => {
        const today = new Date();
        const rangeDate = today.setDate(today.getDate() - 30);
        const last = new Date(rangeDate);
        return new Date(movement.date) > last;
      })
          .map(movement => movement.amount);
    },
    totalAmount() {
      return this.amounts.reduce((total, amount) => total + amount, 0);
    },
  },
  methods: {
    select(el) {
      this.amount = el;
    },
    deleteMovement(id) {
      this.movements = this.movements.filter(movement => movement.id !== id);
      this.saveMovement();
    },
    addMovement(movement) {
      this.movements.push(movement);
      this.saveMovement();
    },
    saveMovement() {
      localStorage.setItem('movements', JSON.stringify(this.movements));
    },
  }
}
</script>

<style scoped>
.modal {
  position: fixed;
  z-index: 999;
  top: 0;
  left: 0;
}

button {
  color: white;
  font-size: 1.25rem;
  background-color: var(--main-blue);
  border: none;
  width: 100%;
  padding: 24px 60px;
  border-radius: 60px;
  box-sizing: border-box;
}
</style>
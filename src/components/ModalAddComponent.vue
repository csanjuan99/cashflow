<template>
  <div class="modal bg-white w-screen h-screen p-3">
    <div class="flex items-center justify-between">
      <h4 class="text-2xl font-bold text-main-blue">Nuevo movimiento</h4>
      <button @click="close"><img src="@/assets/close-icon.svg" alt="Close"></button>
    </div>
    <form class="my-5" @submit.prevent="addMovement">
      <div class="form-group flex flex-col mt-5">
        <label for="title" class="text-lg">Titulo</label>
        <input
            id="title"
            v-model="name"
            class="border-4 rounded border-main-blue p-3"
            placeholder="Titulo"
            type="text"
            required/>
      </div>
      <div class="form-group flex flex-col my-5">
        <label for="date" class="text-lg">Fecha</label>
        <input v-model="date" id="date" type="date" class="border-main-blue">
      </div>
      <div class="form-group flex flex-col my-5">
        <label for="amount" class="text-lg">Monto</label>
        <input
            required
            id="amount"
            v-model="amount"
            type="number"
            class="border-4 rounded border-main-blue p-3"
            placeholder="Monto"/>
      </div>
      <div class="form-group flex flex-col my-5">
        <label for="description" class="text-lg">Descripcion</label>
        <textarea
            type="text"
            id="description"
            v-model="description"
            class="border-4 rounded border-main-blue p-3"
            placeholder="Descripcion"/>
      </div>
      <div class="form-group flex flex-col my-5">
        <label for="movement" class="text-2xl">Tipo de movimiento</label>
        <div id="movement" class="flex flex-col">
          <div class="form-check">
            <input
                id="in"
                v-model="type"
                value="1"
                class="form-check-input appearance-none rounded-full h-4 w-4 border border-gray-300 bg-white checked:bg-blue-200 p-2 checked:border-blue-300 focus:outline-none transition duration-200 mt-1 align-top bg-no-repeat bg-center bg-contain float-left mr-2 cursor-pointer"
                type="radio"
                name="Ingreso"
                checked>
            <label class="form-check-label inline-block text-gray-800" for="in">
              Ingreso
            </label>
          </div>
          <div class="form-check">
            <input
                id="out"
                v-model="type"
                value="2"
                class="form-check-input appearance-none rounded-full h-4 w-4 border border-gray-300 bg-white checked:bg-blue-200 p-2 checked:border-blue-300 focus:outline-none transition duration-200 mt-1 align-top bg-no-repeat bg-center bg-contain float-left mr-2 cursor-pointer"
                type="radio"
                name="Gasto">
            <label class="form-check-label inline-block text-gray-800" for="out">
              Gasto
            </label>
          </div>
        </div>
      </div>
      <button
          type="submit">Agregar movimiento
      </button>
    </form>
  </div>
</template>

<script>
export default {
  name: "ModalAddComponent",
  props: {
    id: {
      type: Number,
      required: true
    }
  },
  data: () => ({
    name: "",
    description: "",
    amount: 0,
    type: 1,
    date: new Date(),
  }),
  methods: {
    addMovement() {
      this.$emit("add", {
        id: this.id,
        name: this.name,
        description: this.description,
        amount: this.amount * (this.type === 1 ? 1 : -1),
        type: this.type,
        date: this.date,
      });
      this.default();
      this.close();
      this.$router.push("/");
    },
    default() {
      this.name = "";
      this.description = "";
      this.amount = 0;
      this.type = 1;
    },
    close() {
      this.$emit("close");
    }
  }
}
</script>

<style scoped>
form > button {
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
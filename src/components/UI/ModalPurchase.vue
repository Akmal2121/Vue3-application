<template>
  <div class="modal" @click="modalvuew2">
    <div class="modal-content" @click.stop="">
      <input
        type="text"
        class="input1"
        placeholder="Category... food, other, transport, clothes"
        v-model="category"
      />
      <input
        type="text"
        class="input1"
        placeholder="Price..."
        v-model="price"
      />
      <input type="date" class="input1" placeholder="Date..." v-model="date" />
      <button @click="send">Send</button>
    </div>
  </div>
</template>

<script>
export default {
  name: "ModalPurchase",
  data() {
    return {
      category: "",
      price: "",
      date: "",
    };
  },
  methods: {
    modalvuew2() {
      const exit = false;
      this.$emit("modalvuew2", exit);
    },
    send() {
      let obj = {
        id: Date.now(),
        price: +this.price,
        category: this.category,
        date: this.date,
      };
      const exit = false;
      obj = JSON.stringify(obj);
      localStorage.setItem("object1", obj);
      this.$emit("purchased", { obj, exit });
    },
  },
};
</script>

<style scoped>
.modal {
  position: absolute;
  top: 0%;
  bottom: 0;
  right: 0;
  left: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
}
.modal-content {
  width: 500px;
  height: 300px;
  background-color: white;
  border-radius: 10px;
  padding: 10px;
  display: flex;
  flex-direction: column;
  justify-content: space-around;
}
.input1 {
  padding: 5px;
  border-radius: 5px;
  border: 1px solid;
}
.input1:focus {
  outline: none;
}
button {
  padding: 10px;
  width: 100px;
  justify-self: center;
  align-self: center;
  cursor: pointer;
}
</style>

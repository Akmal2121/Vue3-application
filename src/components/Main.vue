<template>
  <Header
    @spendTitle="spendTitle"
    :titleTexts="titleTexts"
    :totalSpending="expenses"
    @getTitle="getTitle"
  ></Header>
  <main>
    <div class="btn-wrapper">
      <ModalSalary
        v-if="modal"
        @modalvuew="modalvuew"
        @salared="salared"
      ></ModalSalary>
      <ModalPurchase
        v-if="modal2"
        @modalvuew2="modalvuew2"
        @purchased="purchased"
      ></ModalPurchase>
      <button @click="salary">Доход</button>
      <button @click="purchase">Покупка</button>
    </div>
    <div class="wrapper">
      <div class="main-wrapper">
        <h1>{{ titleText }}</h1>
        <h1 v-if="titleText == 'Общий расход'">{{ totalSpend }}$</h1>
        <h1 v-if="titleText == 'Общий доход'">{{ totalSalary }}$</h1>
        <h1 v-if="titleText == 'Самый большой расход'">{{ totalMaxSpend }}$</h1>
        <h1 v-if="titleText == 'Общий баланс по доходам и расходам'">
          {{ totalBalance }}$
        </h1>
        <h1 v-if="titleText == 'Общий расход на каждую из 4 категорий'"></h1>
        <ul class="ul-category">
          <li v-for="(item, index) in totalCategory" :key="index">
            {{ item }}$
          </li>
        </ul>
        <h1 v-if="titleText == 'Самый большой расход из категории'">
          {{ maxCategorySpend }}$
        </h1>
        <h1 v-if="titleText == ''">Нажмите на конкретную вкладку</h1>
      </div>
    </div>
    <Footer></Footer>
  </main>
</template>

<script>
import Footer from "./Footer.vue";
import ModalSalary from "./UI/ModalSalary.vue";
import ModalPurchase from "./UI/ModalPurchase.vue";
import Header from "@/components/Header.vue";

export default {
  name: "Main",
  components: { Footer, ModalSalary, ModalPurchase, Header },
  data() {
    return {
      modal: false,
      modal2: false,
      income: [
        {
          id: 1,
          price: 3000,
          date: Date.now(),
          comment: "Доход от продаж мебели",
        },
        {
          id: 2,
          price: 5000,
          date: Date.now(),
          comment: "Доход от продаж мебели",
        },
        {
          id: 3,
          price: 1000,
          date: Date.now(),
          comment: "Доход от продаж мебели",
        },
      ],
      expenses: [
        { id: 1, price: 3000, date: Date.now(), category: "food" },
        { id: 2, price: 4000, date: Date.now(), category: "clothes" },
        { id: 5, price: 7000, date: Date.now(), category: "clothes" },
        { id: 3, price: 5000, date: Date.now(), category: "transport" },
        { id: 4, price: 6000, date: Date.now(), category: "other" },
        { id: 6, price: 1000, date: Date.now(), category: "other" },
      ],
      titleTexts: [
        "Общий расход",
        "Общий доход",
        "Самый большой расход",
        "Общий баланс по доходам и расходам",
        "Общий расход на каждую из 4 категорий",
        "Самый большой расход из категории",
      ],
      totalCategory: [],

      titleText: "",
      totalSpend: 0,
      totalSalary: 0,
      totalBalance: 0,
      totalMaxSpend: 0,
      totalFood: 0,
      totalClothes: 0,
      totalTransport: 0,
      totalOther: 0,
      maxCategorySpend: 0,
      categoryPrices: [],
    };
  },
  methods: {
    modalvuew(exit) {
      this.modal = exit;
    },
    salary() {
      this.modal = true;
    },
    purchase() {
      this.modal2 = true;
    },
    modalvuew2(exit) {
      this.modal2 = exit;
    },
    salared(obj) {
      const newOb2 = JSON.parse(localStorage.getItem("object2"));
      this.income.push(newOb2);
      this.modal = obj.exit;
    },
    purchased(obj) {
      const newOb = JSON.parse(localStorage.getItem("object1"));
      this.expenses.push(newOb);
      this.modal2 = obj.exit;
    },
    getTitle(index) {
      let ras = 0;
      let dox = 0;
      this.titleText = this.titleTexts[index];
      this.totalSpend = 0;
      this.totalSalary = 0;
      this.totalBalance = 0;
      if (this.titleText == "Общий расход") {
        this.totalCategory = [];
        this.expenses.forEach((em) => (this.totalSpend += em.price));
        ras = this.totalSpend;
      }
      if (this.titleText == "Общий доход") {
        this.totalCategory = [];
        this.income.forEach((em) => (this.totalSalary += em.price));
        dox = this.totalSalary;
      }
      if (this.titleText == "Общий баланс по доходам и расходам") {
        this.totalCategory = [];
        this.totalMaxSpend = 0;
        ras = 0;
        dox = 0;
        this.expenses.forEach((em) => (this.totalSpend += em.price));
        ras = this.totalSpend;
        this.income.forEach((em) => (this.totalSalary += em.price));
        dox = this.totalSalary;
        this.totalBalance = ras - dox;
      }
      if (this.titleText == "Самый большой расход") {
        this.totalCategory = [];

        this.expenses.forEach((em) => {
          if (this.totalMaxSpend < em.price) {
            this.totalMaxSpend = em.price;
          }
        });
      }
      if (this.titleText == "Общий расход на каждую из 4 категорий") {
        this.totalClothes = 0;
        this.totalFood = 0;
        this.totalTransport = 0;
        this.totalOther = 0;
        this.totalCategory = [];
        this.categoryPrices = [];

        for (let i = 0; i < this.expenses.length; i++) {
          if (this.expenses[i].category == "clothes") {
            this.totalClothes += this.expenses[i].price;
          }
          if (this.expenses[i].category == "food") {
            this.totalFood += this.expenses[i].price;
          }
          if (this.expenses[i].category == "other") {
            this.totalOther += this.expenses[i].price;
          }
          if (this.expenses[i].category == "transport") {
            this.totalTransport += this.expenses[i].price;
          }
        }
        this.totalCategory.push(`food-${this.totalFood}`);
        this.totalCategory.push(`clothes-${this.totalClothes}`);
        this.totalCategory.push(`transport-${this.totalTransport}`);
        this.totalCategory.push(`other-${this.totalOther}`);
        this.categoryPrices.push({ category: "food", price: this.totalFood });
        this.categoryPrices.push({
          category: "transport",
          price: this.totalTransport,
        });
        this.categoryPrices.push({ category: "other", price: this.totalOther });
        this.categoryPrices.push({
          category: "clothes",
          price: this.totalClothes,
        });
      }

      if (this.titleText == "Самый большой расход из категории") {
        this.totalClothes = 0;
        this.totalFood = 0;
        this.totalTransport = 0;
        this.totalOther = 0;
        this.totalCategory = [];
        this.categoryPrices = [];

        for (let i = 0; i < this.expenses.length; i++) {
          if (this.expenses[i].category == "clothes") {
            this.totalClothes += this.expenses[i].price;
          }
          if (this.expenses[i].category == "food") {
            this.totalFood += this.expenses[i].price;
          }
          if (this.expenses[i].category == "other") {
            this.totalOther += this.expenses[i].price;
          }
          if (this.expenses[i].category == "transport") {
            this.totalTransport += this.expenses[i].price;
          }
        }
        this.totalCategory.push(`food-${this.totalFood}`);
        this.totalCategory.push(`clothes-${this.totalClothes}`);
        this.totalCategory.push(`transport-${this.totalTransport}`);
        this.totalCategory.push(`other-${this.totalOther}`);
        this.categoryPrices.push({ category: "food", price: this.totalFood });
        this.categoryPrices.push({
          category: "transport",
          price: this.totalTransport,
        });
        this.categoryPrices.push({ category: "other", price: this.totalOther });
        this.categoryPrices.push({
          category: "clothes",
          price: this.totalClothes,
        });

        for (let i = 0; i < this.categoryPrices.length; i++) {
          if (this.categoryPrices[i].price > this.maxCategorySpend) {
            this.maxCategorySpend = this.categoryPrices[i].price;
          }
        }

        this.totalCategory = [];
      }
    },
  },
  computed: {
    local() {
      return this.expenses.push(localStorage.getItem("object1"));
    },
    local2() {
      return this.income.push(localStorage.getItem("object2"));
    },
  },
};
</script>

<style scoped>
main * {
  margin: 0;
  padding: 0;
}
main {
  background-color: antiquewhite;
  min-height: calc(91.5vh);
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.btn-wrapper {
  display: flex;
  justify-content: center;
}
.btn-wrapper button {
  padding: 10px;
  border-radius: 5px;
  background-color: bisque;
  border: 0;
  cursor: pointer;
  margin: 30px 5px 0;
}
.wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
}
.main-wrapper {
  width: 500px;
  height: 300px;
  background-color: gold;
  border-radius: 20px;
  padding: 10px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
}
</style>

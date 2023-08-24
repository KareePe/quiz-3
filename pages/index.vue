<template>
  <div
    class="flex min-h-full flex-1 flex-col justify-center px-6 py-12 lg:px-8"
  >
    <div class="sm:mx-auto sm:w-full sm:max-w-sm">
      <h2 class="text-base font-semibold leading-7 text-gray-900">Quiz #3</h2>
      <p class="mt-1 text-sm leading-6 text-gray-600">
        ให้เขียนโปรแกรมคำนวณการซื้อของที่ร้านค้า <br />
        โดยหาว่าควรเลือกซื้อร้านไหนคุ้มกับเงินที่เรามีมากที่สุด โดยจะต้องมี
        input จำนวณเงินที่เรามีอยู่ และเงื่อนไขร้านค้ามีตังนี้ <br /><br />

        ร้านที่ 1 ราคาสินค้าชิ้นละ 25 บาท เมื่อซื้อ 10 ชิ้นขึ้นไปลดให้ 20%
        <br />
        ร้านที่ 2 ราคาสินค้าชิ้นละ 30 บาท เมื่อซื้อสินค้า 3 ชิ้น แถมให้ 1 ชิ้น
      </p>
    </div>

    <div class="mt-10 sm:mx-auto sm:w-full sm:max-w-sm">
      <!-- <form class="space-y-6" action="#" method="POST"> -->
      <div>
        <label
          for="price"
          class="block text-sm font-medium leading-6 text-gray-900"
          >จำนวณเงิน</label
        >
        <div class="mt-2">
          <input
            id="price"
            name="price"
            type="number"
            v-model="budget"
            required=""
            class="block w-full rounded-md border-0 py-1.5 text-gray-900 shadow-sm ring-1 ring-inset ring-gray-300 placeholder:text-gray-400 focus:ring-2 focus:ring-inset focus:ring-indigo-600 sm:text-sm sm:leading-6"
          />
        </div>
      </div>

      <div class="mt-2">
        <button
          type="submit"
          class="flex w-full justify-center rounded-md bg-indigo-600 px-3 py-1.5 text-sm font-semibold leading-6 text-white shadow-sm hover:bg-indigo-500 focus-visible:outline focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600"
          @click="calculate()"
        >
          คำนวณ
        </button>
      </div>
      <!-- </form> -->

      <div class="mt-2">
        <ul>
          <li v-for="(result, index) in resultStore" :key="index">
            ร้านที่ {{ result.store }} ได้ {{ result.quantity }} ชิ้น,
            เหลือเงิน: {{ result.remainingBudget }} บาท
          </li>
        </ul>
        <p class="mt-10 text-center text-sm text-gray-500">
          แนะนำให้ซื้อ
          {{ " " }}
          <span
            class="font-semibold leading-6 text-indigo-600 hover:text-indigo-500"
            >ร้านที่ {{ bestStore }} จะคุ้มที่สุด</span
          >
        </p>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";

const budget = ref(0);
let resultStore = ref([]);
let bestStore = ref(null);

const calculate = () => {
  let result = [];
  const store1Price = 25;
  const store2Price = 30;

  const store1Quantity = Math.floor(budget.value / store1Price);
  const store1RemainingBudget =
    budget.value -
    store1Price * store1Quantity * (store1Quantity > 10 ? 0.8 : 1);
  result.push({
    store: 1,
    quantity: store1Quantity,
    remainingBudget: store1RemainingBudget,
  });

  const store2Quantity = Math.floor(budget.value / store2Price);
  const store2FreeItems = Math.floor(store2Quantity / 3);
  const totalQuantityStore2 = store2Quantity + store2FreeItems;

  const store2RemainingBudget = budget.value - store2Price * store2Quantity;
  result.push({
    store: 2,
    quantity: totalQuantityStore2,
    remainingBudget: store2RemainingBudget,
  });

  bestStore = store1RemainingBudget > store2RemainingBudget ? 1 : 2;
  resultStore.value = result;
};
</script>

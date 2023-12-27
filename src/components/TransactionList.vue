<template>
  <h3>History</h3>
  <!-- <ul id="list" class="list"> -->
  <!-- <li
      v-for="transaction in transactions"
      :key="transaction.id"
      :class="transaction.amount < 0 ? 'minus' : 'plus'"
    >
      {{ transaction.text }}<span>{{ transaction.amount }}</span
      ><button @click="deleteTransaction(transaction.id)" class="delete-btn">
        x
      </button>
    </li> -->
  <!-- <li class="minus">
      Cash <span>-$400</span><button class="delete-btn">x</button>
    </li>
    <li class="plus">
      PayCheck<span>$800</span><button class="delete-btn">x</button>
    </li> -->
  <!-- </ul> -->

  <!-- <draggable v-model="$props.transactions" item-key="id" tag="ul"> -->
  <draggable v-model="data" item-key="id" tag="ul" class="list" id="list">
    <template #item="{ element }">
      <li :class="element.amount < 0 ? 'minus' : 'plus'">
        {{ element.text }}<span>{{ element.amount }}</span>
        <button @click="deleteTransaction(element.id)" class="delete-btn">
          x
        </button>
      </li>
    </template>
  </draggable>
</template>

<script setup>
// const Transactions = [
//   { id: 1, text: "Flower", amount: -20 },
//   { id: 2, text: "Salary", amount: 300 },
//   { id: 3, text: "Book", amount: -10 },
//   { id: 4, text: "Camera", amount: 150 },
// ];

import { defineProps, defineEmits, ref, reactive } from "vue";
import draggable from "vuedraggable";
const emit = defineEmits(["transactionDeleted"]);
const props = defineProps({
  transactions: {
    type: Array,
    required: true,
  },
});
const deleteTransaction = (id) => {
  emit("transactionDeleted", id);
};
const data = ref(props.transactions);
</script>

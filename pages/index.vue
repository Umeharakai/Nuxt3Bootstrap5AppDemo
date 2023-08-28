<template>
  <div>
    <!-- ナビゲーションバー -->
    <nav class="bg-dark text-white">
      <ul class="list-inline">
        <li class="list-inline-item"><Nuxt-link to="/">財務会計アプリ</nuxt-link></li>
        <li class="list-inline-item"><Nuxt-link to="/ImagePage">複数ページ</nuxt-link></li>
      </ul>
    </nav>

    <!-- アプリのUIコンポーネント -->
    <p class="text-primary">残高</p>
    <p>{{ state.balance }}</p>
    <input type="number" v-model="state.transactionAmount" placeholder="金額を入力してください" class="form-control mt-2" />
    <button @click="addTransaction" class="btn btn-primary mt-2">入金</button>
    <button @click="withdrawTransaction" class="btn btn-primary mt-2">引き出し</button>

    <!-- 履歴 -->
    <h2 class="text-primary mt-2">履歴</h2>
    <ul>
      <li v-for="transaction in state.transactionHistory" :key="transaction.id">
        {{ transaction.description }} - {{ transaction.amount }}
      </li>
    </ul>
    <Nuxt-link to="/ImagePage">次へ</Nuxt-link>
  </div>
</template>

<script setup>
import { reactive } from 'vue';
import ImagePage from './ImagePage.vue';

const state = reactive({
  balance: 0,
  transactionAmount: 0,
  transactionHistory: [],
});

const addTransaction = () => {
  // 入金処理
  const transaction = {
    id: Date.now(),
    description: '入金',
    amount: state.transactionAmount,
  };
  state.transactionHistory.push(transaction);
  state.balance += state.transactionAmount;
  state.transactionAmount = 0;
};

const withdrawTransaction = () => {
  // 引き出し処理
  if (state.transactionAmount > state.balance) {
    alert('残高が不足しています');
    return;
  }

  const transaction = {
    id: Date.now(),
    description: '引き出し',
    amount: -state.transactionAmount, // 引き出しは負の値として表現する
  };
  state.transactionHistory.push(transaction);
  state.balance -= state.transactionAmount;
  state.transactionAmount = 0;
};
</script>
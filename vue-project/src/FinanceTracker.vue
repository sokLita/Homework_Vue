<template>
  <div class="tracker">
    <h1>Personal Finance Tracker</h1>

    <!-- Add Transaction Form -->
    <div class="form">
      <input v-model="desc" placeholder="Description" />
      <input v-model.number="amount" type="number" placeholder="Amount" />
      <select v-model="type">
        <option value="income">Income</option>
        <option value="expense">Expense</option>
      </select>
      <button @click="addTransaction">Add</button>
      <button @click="clearAll">Clear All</button>
    </div>

    <!-- Filter -->
    <div class="filter">
      <label>Filter: </label>
      <select v-model="filterType">
        <option value="all">All</option>
        <option value="income">Income</option>
        <option value="expense">Expense</option>
      </select>
    </div>

    <!-- Summary -->
    <div class="summary">
      <p>Total Income: ${{ totalIncome }}</p>
      <p>Total Expenses: ${{ totalExpenses }}</p>
      <p>Balance: ${{ balance }}</p>
      <p>Expense %: {{ expensePercentage }}%</p>
      <p>Status: {{ budgetStatus }}</p>
      <p v-if="isOverBudget" style="color:red">Over Budget!</p>
    </div>

    <!-- Budget Progress Bar -->
    <div class="progress-bar-wrapper">
      <label>Budget Used: {{ expensePercentage }}%</label>
      <div class="progress-bar">
        <div
          class="progress-fill"
          :style="{
            width: expensePercentage + '%',
            backgroundColor: isOverBudget ? 'red' : 'green'}"
        ></div>
      </div>
    </div>

    <!-- Notification Log -->
    <div class="notifications" v-if="notificationLog.length">
      <h3>Notifications</h3>
      <ul>
        <li v-for="(log, index) in notificationLog" :key="index">{{ log }}</li>
      </ul>
    </div>

    <!-- Transaction List -->
    <div class="transactions">
      <h3>Transactions</h3>
      <ul>
        <li v-for="tran in filteredTransactions" :key="tran.id">
          <span>{{ tran.date }} | {{ tran.desc }} | ${{ tran.amount }} | {{ tran.type }}</span>
          <button @click="deleteTransaction(tran.id)">Delete</button>
        </li>
      </ul>
    </div>

    <!-- Category Summary -->
    <div class="category">
      <h3>Category Summary</h3>
      <ul>
        <li v-for="(total, type) in categorySummary" :key="type">
          {{ type }}: ${{ total }}
        </li>
      </ul>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

// ============================================
// REACTIVE STATE
// ============================================
const transactions = ref([])   // list of all transactions
const filterType = ref('all')  // filter: 'all' | 'income' | 'expense'
const budgetLimit = ref(1000)  // budget limit
const notificationLog = ref([]) // notification messages

// Form input state
const desc = ref('')
const amount = ref(0)
const type = ref('income')

// ============================================
// COMPUTED PROPERTIES (7)
// ============================================

// filteredTransactions — filter by type
const filteredTransactions = computed(() => {
  if (filterType.value === 'all') return transactions.value
  return transactions.value.filter(t => tran.type === filterType.value)
})

// totalIncome — sum of all income
const totalIncome = computed(() => {
  return transactions.value
    .filter(tran => tran.type === 'income')
    .reduce((sum, tran) => sum + tran.amount, 0)
})

// totalExpenses — sum of all expenses
const totalExpenses = computed(() => {
  return transactions.value
    .filter(tran=> tran.type === 'expense')
    .reduce((sum, tran) => sum + tran.amount, 0)
})

// balance — income minus expenses
const balance = computed(() => {
  return totalIncome.value - totalExpenses.value
})

// isOverBudget — true if expenses exceed limit
const isOverBudget = computed(() => {
  return totalExpenses.value > budgetLimit.value
})

// expensePercentage — capped at 100%
const expensePercentage = computed(() => {
  if (budgetLimit.value === 0) return 0
  const percent = (totalExpenses.value / budgetLimit.value) * 100
  return Math.min(Math.round(percent), 100)
})

// categorySummary — grouped totals by type
const categorySummary = computed(() => {
  const summary = {}
  transactions.value.forEach(tran => {
    if (!summary[tran.type]) summary[tran.type] = 0
    summary[tran.type] += tran.amount
  })
  return summary
})

// budgetStatus — label string
const budgetStatus = computed(() => {
  if (expensePercentage.value >= 100) return 'Over Budget'
  if (expensePercentage.value >= 75)  return 'Warning'
  return 'Good'
})

// ============================================
// METHODS
// ============================================

// addTransaction — add new transaction
function addTransaction(descVal, amountVal, typeVal) {
  if (!desc.value || amount.value <= 0) return
  transactions.value.push({
    id: Date.now(),
    desc: desc.value,
    amount: amount.value,
    type: type.value,
    date: new Date().toLocaleDateString()
  })
  // reset form
  desc.value = ''
  amount.value = 0
  type.value = 'income'
}

// deleteTransaction — remove by id
function deleteTransaction(id) {
  transactions.value = transactions.value.filter(tran => tran.id !== id)
}

// clearAll — remove all transactions
function clearAll() {
  transactions.value = []
  notificationLog.value = []
}

// ============================================
// WATCHERS
// ============================================

// watch transactions → save to localStorage
watch(transactions, (newVal) => {
  localStorage.setItem('transactions', JSON.stringify(newVal))
}, { deep: true })

// watch balance → warning if balance < 0
watch(balance, (newVal) => {
  if (newVal < 0) {
    notificationLog.value.push(`Balance is negative: $${newVal}`)
  }
})

// watch isOverBudget → log notification
watch(isOverBudget, (newVal) => {
  if (newVal) {
    notificationLog.value.push(`Over budget! Spent $${totalExpenses.value} of $${budgetLimit.value}`)
  }
})
</script>

<style scoped>
.tracker { 
    max-width: 600px; margin: auto; font-family: Arial; padding: 20px; 
}
.form, .filter, .summary, .transactions, .category {
     margin-bottom: 20px; 
}
.form input, .form select, .form button { 
    margin: 5px; padding: 8px; 
}
.progress-bar-wrapper { 
    margin-bottom: 20px; 
}
.progress-bar { 
    width: 100%; height: 20px; background: #eee; border-radius: 10px; 
}
.progress-fill { 
    height: 20px; border-radius: 10px; transition: width 0.3s; 
}
button { 
    cursor: pointer; padding: 5px 10px; 
}
ul { 
    list-style: none; padding: 0; 
}
li { 
    display: flex; justify-content: space-between; padding: 5px 0; border-bottom: 1px solid #eee; 
}
</style>
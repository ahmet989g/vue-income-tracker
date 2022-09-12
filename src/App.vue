<template>
  <app-header :totalIncome="state.totalIncome" />
  <app-form @add-income="AddIncome" />
  <income-list :state="state" />
</template>

<script>
import AppHeader from './components/AppHeader';
import AppForm from './components/AppForm.vue';
import IncomeList from './components/IncomeList.vue';
import { computed, reactive } from 'vue';

export default {
  
  setup() {
    const state = reactive({
      income: [],
      totalIncome: computed(() => {
        let temp = 0;
        if(state.income.length > 0){
          for(let i = 0; i < state.income.length; i++){
            temp += state.income[i].value;
          }
        }
        return temp;
      }),
      sortedIncome: computed(() => {
        let temp = [];

        temp = state.income.slice().sort(function (a, b) {
          return new Date(b.date) - new Date(a.date);
        });

        return temp;
      })
    });

    function AddIncome(data){
      let d = data.date.split("-");
      let newD = new Date(d[0], d[1], d[2]);

      state.income = [...state.income,{
        id:Date.now(),
        desc: data.desc,
        value: parseInt(data.value),
        date: newD.getTime()
      }];

    }

    return{
      state,
      AddIncome
    }
  },
  components:{
    AppHeader,
    AppForm,
    IncomeList
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Fira Sans', sans-serif;
}

body{
  background: #EEE;
}
</style>

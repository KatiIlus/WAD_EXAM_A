<template>
    <table>
        <tr>
            <td>From / To</td>
            <td id="amount-label" @click="sort('amount')">Amount</td>
            <td>Currency</td>
        </tr>
        <tr v-for="(transaction, index) in sortedTransactions" :key="index">
          <td class="green" v-if="transaction.type==='income'">{{transaction.to}}{{transaction.from}}</td>
          <td class="red" v-if="transaction.type==='spending'">{{transaction.to}}{{transaction.from}}</td>
          <td class="green" v-if="transaction.type==='income'">{{transaction.amount}}</td>
          <td class="red" v-if="transaction.type==='spending'">{{transaction.amount}}</td>
          <td class="green" v-if="transaction.type==='income'">{{transaction.currency}}</td>
          <td class="red" v-if="transaction.type==='spending'">{{transaction.currency}}</td>
        </tr>
        <tr>
            <td colspan="3" style="text-align: right;">
                <span>Balance: <span id="balance"></span>{{balance}} EUR</span>
            </td>
        </tr>
    </table>
</template>

<script>
    export default {
        name: 'Transactions',

        data:function() {
          return {
            currentSort:'amount',
            currentSortDir:'desc'
          };
        },

        props: {
            transactions: Array
        },

        methods:{
          sort:function(s) {
            //used code from https://www.raymondcamden.com/2018/02/08/building-table-sorting-and-pagination-in-vuejs as an example
            //if s == current sort, reverse
            if(s === this.currentSort) {
              this.currentSortDir = this.currentSortDir==='desc'?'asc':'desc';
            }
            this.currentSort = 'amount';
          }
        },

        computed:{
          sortedTransactions:function() {
            //used code from https://www.raymondcamden.com/2018/02/08/building-table-sorting-and-pagination-in-vuejs as an example
            return this.transactions.slice().sort((a,b) => {
              let modifier = 1;
              if(this.currentSortDir === 'desc') modifier = -1;
              if(a[this.currentSort] < b[this.currentSort]) return -1 * modifier;
              if(a[this.currentSort] > b[this.currentSort]) return 1 * modifier;
              return 0;
            });
          },

          balance:function(){
            //used code from https://www.iditect.com/how-to/50616608.html as an example
            let balance = 0;
            for(let i = 0; i < this.transactions.length; i++){
              if(this.transactions[i].type === 'income'){
                balance+= parseFloat(this.transactions[i].amount);
              }
              else if(this.transactions[i].type === 'spending'){
                balance-= parseFloat(this.transactions[i].amount);
              }
            }
            return balance;
          }
        }
    }
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
    table {
        width: 50%;
        margin: 0 auto;
    }
   .green {
        color: green;
   }
   .red {
        color: red;
   }

</style>
<template>
  <div>
      <md-table v-model="operations" md-card>
      <md-table-toolbar>
        <h1 class="md-title">Operacje</h1>
      </md-table-toolbar>

      <md-table-row slot="md-table-row" slot-scope="{ item }">
        <!-- <md-table-cell md-label="ID" md-sort-by="id" md-numeric>{{ item.id }}</md-table-cell> -->
        <md-table-cell md-label="Tytuł" md-sort-by="title">{{ item.title }}</md-table-cell>
        <md-table-cell md-label="Wartość" md-sort-by="value"><span :class="{'income': (item.action == 'in'), 'outcome': (item.action == 'out'),}">{{ formatPrice(item.amount) }}</span></md-table-cell>
        <md-table-cell md-label="Odbiorca/Nadawca" md-sort-by="forginName">{{ item.action == "out" ? item.name_prin :  item.name_ben}}</md-table-cell>
        <md-table-cell md-label="NRB Odbiorca/Nadawca" md-sort-by="NRB">{{ item.action == "out" ? item.nrb_prin :  item.nrb_ben}}</md-table-cell>
        <md-table-cell md-label="Data" md-sort-by="date">{{ item.realisation_date }}</md-table-cell>
      </md-table-row>
    </md-table>
  </div>
</template>

<script>
import axios from "axios"

export default {
  props: {
    id: Number
  },
  data() {
      return {
        operations: [],
        page: 1,
        total: 0,
        perPage: 10
      }
  },
  async mounted() {
    await this.getOperations();
  },
  methods: {
    async getOperations() {
      let newOperations = await axios.get("http://localhost:8000/api/transactions/"+this.id+"?page="+this.page);

      newOperations.data.data.forEach(element => {
        if(element.ben_banking_account_id == this.id) {
          element['action'] = "in";
        } else {
          element['action'] = "out";
          element['amount'] = -element['amount'];
        }
        this.operations.push(element);
      });

      this.total = newOperations.data.total;
      this.perPage = newOperations.data.perPage;  
    },
    formatPrice(value) {
        let val = (value/1).toFixed(2).replace('.', ',')
        return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")
    }
  }
}
</script>

<style>
.income {
    color: green;
}
.outcome {
    color: red;
}

</style>
<template>
  <div>
      <md-table v-model="operations" md-card>
      <md-table-toolbar>
        <h1 class="md-title">Operacje</h1>
      </md-table-toolbar>

      <md-table-row slot="md-table-row" slot-scope="{ item }">
        <!-- <md-table-cell md-label="ID" md-sort-by="id" md-numeric>{{ item.id }}</md-table-cell> -->
        <md-table-cell md-label="Tytuł" md-sort-by="title">{{ item.title }}</md-table-cell>
        <md-table-cell md-label="Wartość" md-sort-by="value"><span :class="{'income': (item.action == 'in'), 'outcome': (item.action == 'out'),}">{{ item.value }}</span></md-table-cell>
        <md-table-cell md-label="Odbiorca/Nadawca" md-sort-by="forginName">{{ item.forginName }}</md-table-cell>
        <md-table-cell md-label="Data" md-sort-by="date">{{ item.date }}</md-table-cell>
      </md-table-row>
    </md-table>
  </div>
</template>

<script>
export default {
    props: {
        operations: Array
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
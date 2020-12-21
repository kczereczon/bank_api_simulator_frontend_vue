<template>
<div >
  <div id="home" class="md-layout md-gutter md-alignment-top-center">

        <div class="md-layout-item md-size-100">
        <BankingAccounts :id="id" @bankingAccountChanged="changeBankingAccount"/>
        </div>
        <div class="md-layout-item md-size-100">
        <BalanceInformations :balance="balance[id]"/>
      </div>
      <div class="md-layout-item md-size-100">
        <Operations :operations="operations[id]"/>
      </div>
  </div>
  </div>
</template>

<style>
  #home > .md-layout-item {
    margin-top: 8px;
    margin-bottom: 8px;
  }
</style>

<script>
import BankingAccounts from "../components/BankingAccounts"
import BalanceInformations from "../components/BalanceInformations"
import Operations from "../components/Operations"

export default {
  data() {
    return {
      operations: {1: [
                {
                    title: "Wynajem kawalerki",
                    value: "-1500 zł",
                    action: "out",
                    forginName: "Julia Pawlikowska",
                    date: "2020-12-10"
                },
                {
                    title: "Wypłata Umowa o Prace",
                    value: "5000 zł",
                    action: "in",
                    forginName: "Testowa firma Z.O.O",
                    date: "2020-12-10"
                },{
                    title: "Wynajem samochodu",
                    value: "-3000 zł",
                    action: "out",
                    forginName: "Jan Kowalski",
                    date: "2020-11-25"
                }
            ],
            2: [
                {
                    title: "Opłata za mieszkanie - grudzień",
                    value: "-800 zł",
                    action: "out",
                    forginName: "Jan Kowalski",
                    date: "2020-12-10"
                },
                {
                    title: "Umowa zlecenie - grudzień",
                    value: "6000 zł",
                    action: "in",
                    forginName: "Testowa firma Z.O.O",
                    date: "2020-12-10"
                },{
                    title: "Opłata za mieszkanie - październik",
                    value: "-800 zł",
                    action: "out",
                    forginName: "Jan Kowalski",
                    date: "2020-11-10"
                }
            ]},
        balance: {
          1: "500 zł",
          2: "4400 zł"
        },
        id: 1
    }
  },
  name: 'Home',
  components: {
    BankingAccounts,
    BalanceInformations,
    Operations
  },
  methods: {
    changeBankingAccount: function (id) {

      this.id = id;
    }
  }
}
</script>

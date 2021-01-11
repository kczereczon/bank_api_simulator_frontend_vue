<template>
  <div>
    <div id="home" class="md-layout md-gutter md-alignment-top-center">
      <div class="md-layout-item md-size-100">
        <BankingAccounts
          :bankingAccounts="bankingAccounts"
          :id="id"
          @bankingAccountChanged="changeBankingAccount"
        />
      </div>
      <div class="md-layout-item md-size-100">
        <BalanceInformations :balance="balance[id]" />
      </div>
      <div v-if="id" class="md-layout-item md-size-100">
        <Operations :id="id" />
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
import BankingAccounts from "../components/BankingAccounts";
import BalanceInformations from "../components/BalanceInformations";
import Operations from "../components/Operations";
import axios from "axios";

export default {
  async mounted() {
    await this.getBankingInfo(JSON.parse(localStorage.getItem("logged")).id);
  },
  data() {
    return {
      isGeneral: 0,
      bankingAccounts: {},
      balance: {},
      id: 0,
    };
  },
  name: "Home",
  components: {
    BankingAccounts,
    BalanceInformations,
    Operations,
  },
  methods: {
    changeBankingAccount: function (id) {
      this.id = id;
    },
    getBankingInfo: async function (id) {
      try {
        let response = await axios.get("http://localhost:8000/api/user/" + id);
        response.data.banking_accounts.forEach((element) => {
          if (!this.id) {
            this.id = element.id;
          }
          if (element.general_account) {
            this.isGeneral = element.general_account;
            localStorage.setItem('isAdmin', element.general_account);
          }
          this.bankingAccounts[element.id] = element;
          this.balance[element.id] = element.balance;
          
        });
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<template>
    <div>
  <form @submit.prevent="validateUser">
      <md-card>
          <md-progress-bar md-mode="indeterminate" v-if="sending" />
        <md-card-header>
          <div class="md-title">Konta bankowe</div>
        </md-card-header>

        <md-card-content>
            <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-50">
                <span class="md-body-1"><md-icon>person</md-icon> {{user.name}}</span>
            </div>
            <div class="md-layout-item md-small-size-50">
                <span class="md-body-1"><md-icon>email</md-icon> {{user.email}}</span>
            </div>
          </div>
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field>
                <label for="banking-account">Konto bankowe</label>
                <md-select name="bankingAccount" id="bankingAccount" v-model="form.bankingAccount" md-dense :disabled="sending">
                  <md-option value="1">73109024021872653386233218</md-option>
                  <md-option value="2">93109024022648527338391116</md-option>
                </md-select>
              </md-field>
            </div>
          </div>
        </md-card-content>
            
      </md-card>


      <md-snackbar :md-active.sync="bankingAccountChanged">Użytkownik zmienił konto bankowe!</md-snackbar>
    </form>
  </div>
</template>

<script>
export default {
    name: 'BankingAccountsForm',
    data: () => ({
      form: {
        bankingAccount: 1,
      },
      bankingAccountChanged: false,
      sending: false,
      user: JSON.parse(localStorage.getItem('logged'))
    }),
    methods: {
      validateUser () {
        this.$v.$touch()

        if (!this.$v.$invalid) {
          this.saveUser()
        }
      }
    },
    watch: {
        "form.bankingAccount": function(newValue) {
            this.sending = true

        // Instead of this timeout, here you can call your API
            window.setTimeout(() => {
            this.bankingAccountChanged = true
            this.sending = false
            this.form.bankingAccount = newValue;
            this.$emit("bankingAccountChanged", newValue);
            }, 500)
        }
    }
  }
</script>

<style>

</style>
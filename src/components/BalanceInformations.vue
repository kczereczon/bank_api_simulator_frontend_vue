<template>
  <div>
      <md-card >
        <md-card-header>
          <div class="md-title">Saldo konta</div>
        </md-card-header>

        <md-card-content>
          <div class="md-layout md-alignment-top-center md-gutter">
            <div class="md-layout-item">
                <span class="md-title"><md-icon>money</md-icon> {{formatPrice(balance)}} zł</span>
            </div>
          </div>
        </md-card-content>
            
      </md-card>
  </div>
</template>

<script>
export default {
  props: {
    balance: Number
  },
  methods: {
    formatPrice(value) {
        let val = (value/1).toFixed(2).replace('.', ',')
        return val.toString().replace(/\B(?=(\d{3})+(?!\d))/g, " ")
    }
  }
}
</script>

<style>

</style>
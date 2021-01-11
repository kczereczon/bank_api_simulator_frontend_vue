<template>
  <div>
    <md-button class="md-raised md-primary" @click="onClick"
      >Nowy przelew</md-button
    >
    <md-dialog :md-active.sync="showDialog">
      <form novalidate class="md-layout" @submit.prevent="validateTransfer">
        <md-card class="md-layout-item md-size-100 md-small-size-100">
          <md-card-header>
            <div class="md-title">Przelew</div>
          </md-card-header>

          <md-card-content>
              <md-field :class="getValidationClass('title')">
              <label for="title">Tytuł</label>
              <md-input
                type="title"
                name="title"
                id="title"
                autocomplete="title"
                v-model="form.title"
                :disabled="sending"
              />
              <span class="md-error" v-if="!$v.form.title.required"
                >The title is required</span
              >
            </md-field>

            <div class="md-layout md-gutter">
              <div class="md-layout-item md-small-size-100">
                <md-field :class="getValidationClass('firstName')">
                  <label for="first-name">First Name</label>
                  <md-input
                    name="first-name"
                    id="first-name"
                    autocomplete="given-name"
                    v-model="form.firstName"
                    :disabled="sending"
                  />
                  <span class="md-error" v-if="!$v.form.firstName.required"
                    >The first name is required</span
                  >
                  <span
                    class="md-error"
                    v-else-if="!$v.form.firstName.minlength"
                    >Invalid first name</span
                  >
                </md-field>
              </div>

              <div class="md-layout-item md-small-size-100">
                <md-field :class="getValidationClass('lastName')">
                  <label for="last-name">Last Name</label>
                  <md-input
                    name="last-name"
                    id="last-name"
                    autocomplete="family-name"
                    v-model="form.lastName"
                    :disabled="sending"
                  />
                  <span class="md-error" v-if="!$v.form.lastName.required"
                    >The last name is required</span
                  >
                  <span class="md-error" v-else-if="!$v.form.lastName.minlength"
                    >Invalid last name</span
                  >
                </md-field>
              </div>
            </div>

            <md-field :class="getValidationClass('nrb')">
              <label for="nrb_ben">nrb_ben</label>
              <md-input
                type="nrb_ben"
                name="nrb_ben"
                id="nrb_ben"
                autocomplete="nrb_ben"
                v-model="form.nrb_ben"
                :disabled="sending"
              />
              <span class="md-error" v-if="!$v.form.nrb_ben.required"
                >The nrb_ben is required</span
              >
            </md-field>

            <md-field :class="getValidationClass('amount')">
              <label for="amount">Amount</label>
              <md-input
                type="amount"
                name="amount"
                id="amount"
                autocomplete="amount"
                v-model="form.amount"
                :disabled="sending"
              />
              <span class="md-error" v-if="!$v.form.amount.required"
                >The amount is required</span
              >
            </md-field>
          </md-card-content>

          <md-progress-bar md-mode="indeterminate" v-if="sending" />

          <md-card-actions>
            <md-button type="submit" class="md-primary" @click="validateTransfer()" :disabled="sending"
              >Wyślij</md-button
            >
            <md-button type="submit" class="md-primary" @click="showDialog = false" :disabled="sending"
              >Anuluj</md-button
            >
          </md-card-actions>
        </md-card>

        <md-snackbar :md-active.sync="transferSent"
          >The transfer for {{ lastUser }} was sent with success!</md-snackbar
        >
      </form>
      <!-- <md-dialog-actions>
        <md-button class="md-primary" @click="showDialog = false"
          >Close</md-button
        >
        <md-button class="md-primary" @click="showDialog = false"
          >Save</md-button
        >
      </md-dialog-actions> -->
    </md-dialog>
  </div>
</template>

<script>
import { validationMixin } from "vuelidate";
import {
  required,
  minLength,
} from "vuelidate/lib/validators";
  import axios from "axios";
export default {
  name: "FormValidation",
  mixins: [validationMixin],
  data: () => ({
    form: {
      firstName: null,
      lastName: null,
      nrb_ben: null,
      amount: null,
      title: null,
    },
    transferSent: false,
    sending: false,
    lastUser: null,
    showDialog: false,
  }),
  validations: {
    form: {
      firstName: {
        required,
        minLength: minLength(3),
      },
      lastName: {
        required,
        minLength: minLength(3),
      },
      nrb_ben: {
        required,
      },
      amount: {
        required,
      },
      title: {
        required,
      },
    },
  },
  props: {
    id: Number,
  },
  methods: {
    onClick() {
      this.showDialog = true;
    },
    getValidationClass(fieldName) {
      const field = this.$v.form[fieldName];

      if (field) {
        return {
          "md-invalid": field.$invalid && field.$dirty,
        };
      }
    },
    clearForm() {
      this.$v.$reset();
      this.form.firstName = null;
      this.form.lastName = null;
      this.form.nrb_ben = null;
      this.form.amount = null;
      this.form.title = null;
    },
    async validateTransfer() {
      this.$v.$touch();

      if (!this.$v.$invalid) {
        this.send();
      }
    },
    async send() {
        this.sending = true;
      try {
        await axios.post("http://localhost:8000/api/transaction/"+this.id, {
          ...this.form,
          name_ben: this.form.firstName + " " + this.form.lastName,
        });
        this.lastUser = `${this.form.firstName} ${this.form.lastName}`;
        this.transferSent = true;
        this.sending = false;
        this.showDialog = false;
        this.$emit("update");
        this.clearForm();
      } catch (error) {
        this.sending = false;
        console.log(error.response);
      }
    }
  },
};
</script>

<style>
</style>
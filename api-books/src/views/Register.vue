<template>
  <div class="register">
    <h1>This is a registration page</h1>
    <p>Tokens present {{ Object.keys(params).length }}</p>
    <div align="center">
      <table cellpadding="10">
        <tr v-for="(paramValue, paramKey) in params" :key="paramKey">
          <th>Key:</th>
          <td>{{ paramKey }}</td>
          <td>&nbsp;</td>
          <th>Value:</th>
          <td>{{ paramValue }}</td>
        </tr>
      </table>
    </div>
    <FormulateForm v-model="formValues">
      <FormulateInput
        type="text"
        name="code"
        label="Client Code"
        disabled
      />
      <FormulateInput
        type="text"
        name="grant_type"
        label="Grant Type"
        disabled
      />
      <FormulateInput
        type="text"
        name="client_id"
        label="Client ID"
      />
      <FormulateInput
        type="text"
        name="client_secret"
        label="Client Secret"
      />
    </FormulateForm>
  </div>
</template>

<script lang="ts">
import Vue from "vue"

export default Vue.extend({
  props: {
    params: {
      type: Object,
      default: null
    }
  },
  data() {
    return {
      /* eslint-disable @typescript-eslint/camelcase */
      formValues: {
        code: this.params.code,
        grant_type: 'authorization_code',
        client_id: '',
        client_secret: ''
      }
    }
  }
})
</script>

<style scoped lang="scss">
@import '~@braid/vue-formulate/themes/snow/snow.scss';
</style>

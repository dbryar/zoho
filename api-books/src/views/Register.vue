<template>
  <div class="register">
    <h1>Client registration page</h1>
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
        <tr v-if="bearerToken">
          <th>Bearer Token</th>
          <td colspan="3">{{ bearerToken }}</td>
        </tr>
        <tr v-if="refreshToken">
          <th>Refresh Token</th>
          <td colspan="3">{{ refreshToken }}</td>
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
        name="grant"
        label="Grant Type"
        disabled
      />
      <FormulateInput
        type="text"
        name="secret"
        label="Client Secret"
      />
      <FormulateInput
        type="button"
        label="Generate Code"
        @click="getCode(registrationUrl)"
      />
      <FormulateInput
        type="button"
        label="Generate Token"
        @click="getToken(tokenUrl)"
      />
    </FormulateForm>
    <p v-if="errorMessage">{{ errorMessage }}</p>
  </div>
</template>

<script>
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
        grant: 'authorization_code',
        secret: ''
      },
      clientId: '1000.34H035JDMXQ8ZGJYAQUNSHMQ7E6XHR',
      scope: 'ZohoBooks.invoices.CREATE,ZohoBooks.invoices.READ,ZohoBooks.invoices.UPDATE,ZohoBooks.contacts.CREATE,ZohoBooks.contacts.UPDATE,ZohoBooks.contacts.READ',
      redirect: 'https://vuejs-storefront.web.app/register',
      bearerToken: '',
      refreshToken: '',
      errorMessage: ''
    }
  },
  computed: {
    registrationUrl() {
      const url = `https://accounts.zoho.com.au/oauth/v2/auth?scope=${this.scope}&client_id=${this.clientId}&state=testing&response_type=code&redirect_uri=${this.redirect}`
      return url
    },
    tokenUrl() {
      const url = `https://accounts.zoho.com/oauth/v2/token?code=${this.formValues.code}&client_id=${this.clientId}&client_secret=${this.formValues.secret}&redirect_uri=${this.redirect}&grant_type=${this.formValues.grant}`
      return url
    }
  },
  methods: {
    getCode(url) {
      window.location.href = url
    },
    getToken(url) {
      const requestOptions = {
        method: 'POST',
        // headers: { 'Content-Type': 'application/json' },
        // body: JSON.stringify({ title: 'Vue POST Request Example' })
      };
      fetch(url, requestOptions)
        .then(async response => {
          console.log(response)
          const data = await response.json()

          // check for error response
          if (!response.ok) {
            // get error message from body or default to response status
            const error = (data && data.message) || response.status
            return Promise.reject(error)
          }

          this.bearerToken = data.access_token
          this.refreshToken = data.refresh_token
        })
        .catch(error => {
          this.errorMessage = error
          console.error('POST error:', error)
        });
    }
  }
})
</script>

<style scoped lang="scss">
@import '~@braid/vue-formulate/themes/snow/snow.scss';
</style>

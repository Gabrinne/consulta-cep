<template>
<div>
    <h1>{{title}}</h1>

    <form @submit.prevent="onSubmit">
        <input type="text" class="text" placeholder="Informe o CEP:" v-model="cep">
        <button type="submit">Buscar</button>
    </form>

      <div v-if="error != ''">
        {{error}}
      </div>
    <div v-if="preloader">
      <img src="../assets/preloader.gif" alt="carregando.." >
      Carregando...
    </div>

    <div v-if="address.bairro">
      <p><b>Bairro: </b>{{ address.bairro }}</p>
      <p><b>Cidade: </b>{{ address.cidade }}</p>
      <p><b>Logradouro: </b>{{ address.logradouro }}</p>
      <p><b>cep: </b>{{ address.cep }}</p>
    </div>

</div>
</template>

<script>
export default {
    data() {
        return {
            title: 'Buscar CEP',
            error: '',
            cep: '',
            address: {},
            preloader: false
        }
    },

    methods: {
        onSubmit() {
          this.reset()
          this.preloader = true

            this.$http.get(`https://api.postmon.com.br/v1/cep/${this.cep}`)
            .then(response => {
                this.address = response.body

            }, error => {
              this.error = 'CEP não existente. tente novamente'
              console.log(error)
            }).finally(() => {
              this.preloader = false
            })
        },
        reset() {
          this.error = ''
          this.address = {}
        }
    }
}
</script>

<style>

</style>

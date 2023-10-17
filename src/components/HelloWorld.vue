<template>
  <v-container>
    <v-row>
      <v-col class="mb-4 d-flex justify-center align-center">
        <h1 class="display-2 font-weight-bold mb-3">
          Welcome to {{ username }}
        </h1>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: 'HelloWorld',

  data: () => ({

    username: ''
  }),
  created () {
    this.$EventBus.$on('grtUsername', this.getUsername)
    this.getUsername()
  },
  methods: {
    getUsername () {
      if (localStorage.getItem('username') !== null) {
        this.username = localStorage.getItem('username')
      } else {
        this.username = 'Vuetify'
      }
    }
  }
}
</script>

<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
     
      <v-spacer></v-spacer>

      <v-btn
       @click="gotoProduct()"
      >
      Check the product

      </v-btn>
      
      <v-btn
       @click="gotoLogin()"
      >
       เข้าสู่ระบบ

      </v-btn>

    </v-app-bar>

    <v-main>
      <router-view/>
    </v-main>
  </v-app>
</template>

<script>

export default {
  name: 'App',

  data: () => ({
    //
  }),
  methods: {
    gotoLogin () {
      this.$router.push({ path: '/login' }).catch(() => {})
    },
    gotoManage () {
      this.$router.push({ path: '/manageTable' }).catch(() => {})
    },
    gotoProduct () {
      this.$router.push({ path: '/product' }).catch(() => {})
    }
  }
}
</script>

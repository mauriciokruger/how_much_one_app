<template>
  <v-app-bar flat>
    <v-row align="center" no-gutters justify="center" justify-md="space-between">
      <div class="text-h4 pl-md-3">
        Quanto custa um aplicativo?
      </div>
      <a href="https://d3t.com.br/" target="_blank">
        <img class="logo" src="../../assets/logo_white.png" />
      </a>
    </v-row>
  </v-app-bar>
</template>

<script setup>
  //
</script>
<style lang="scss">
.logo {
  max-width: 200px;
  padding: 0 15px 10px 0;
  @media (max-width: 767px){
    max-width: 170px;
    padding: 0 0 10px 0;
  }
}
.v-toolbar__content {
  @media (max-width: 767px){
    height: 80px!important;
  }
}
</style>
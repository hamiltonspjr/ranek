<template>
  <header>
    <nav>
      <router-link to="/" class="logo">
        <img src="@/assets/ranek.svg" alt="Ranek">
      </router-link>
      <router-link v-if="$store.state.login" class="btn" :to="{ name: 'user'}">{{name}}</router-link>
      <router-link to="/login" class="btn" v-else>Vender / Login</router-link>
    </nav>
  </header>
</template>

<script>
export default {
  name: 'TheHeader',
  computed: {
    name() {
      return this.$store.state.user.nome.replace(/ .*/, "");
    }
  }
}
</script>

<style scoped>
nav {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 15px 20px;
  box-shadow: 0 2px 4px rgba(30,60,90,0.2);
}
.logo {
  padding: 10px 0;
}
.logo img {
  width: 90px;
}
</style>
<template>
  <nav class="teal lighten-1">
    <div class="container nav-wrapper">
      <router-link to="/" class="brand-logo">Задачи</router-link>
      <ul id="nav-mobile" class="right hide-on-med-and-down">
        <router-link tag="li"
                     to="/"
                     exact
                     active-class="active"
        ><a>Новая задача</a></router-link>
        <router-link tag="li"
                     to="/list"
        ><a>Список задач</a></router-link>
      </ul>
    </div>
  </nav>
</template>

<script>
  export default {}
</script>

<style lang="scss" scoped>

</style>
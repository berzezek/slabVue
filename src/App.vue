<template>
  <UserNavbar />
  <Products />
</template>

<script>
import UserNavbar from './components/userNavbar/UserNavbar'
import Products from './components/Products/Products'

export default {
  name: 'App',
  components: {
    UserNavbar,
    Products,
  }
}
</script>

<style>

</style>

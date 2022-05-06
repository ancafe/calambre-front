<template>
  <div class="container">
    <p v-if="loggedIn">
      Hello {{ user.name }}
    </p>
    <p v-if="!loggedIn">
      Please sign in
    </p>
    <Navbar />
  </div>
</template>

<script>
import Navbar from "~/components/Sidebar/Navbar";
export default {
  name: 'IndexPage',
  components: {Navbar}
}
</script>


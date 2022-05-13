<template>
  <div>
    <Navbar/>
    <AlertEdis/>
    <Nuxt/>
  </div>
</template>

<script>

import Navbar from "~/components/Sidebar/Navbar";
import AlertEdis from "~/components/Sidebar/AlertEdis";

export default {
  components: {AlertEdis, Navbar}
}
</script>

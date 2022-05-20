<template>
  <div class="notification is-danger">
    <div v-for="msg in message" >
      <strong>Error {{ msg.code }}:</strong> {{msg.description}}
      <div v-if="msg.variables">
        <div v-for="des in msg.variables">
          - {{des[0]}}
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Notification',
  props: ['message']
}
</script>

<template>
  <div>
    <h1>Welcome, {{ username }}</h1>

    <input v-model="input" placeholder="Say something" />
    <button @click="sendMessage">Send</button>

    <div v-html="message"></div> <!-- XSS vulnerability -->

    <button @click="deleteUser">Delete User</button> <!-- CSRF + no auth -->
  </div>
</template>

<script>
export default {
  data() {
    return {
      username: this.$route.query.username || 'Guest', // reflected input
      input: '',
      message: ''
    };
  },
  methods: {
    sendMessage() {
      this.message = this.input; // Unsafe DOM rendering (XSS)
    },
    deleteUser() {
      fetch(`https://example.com/api/delete-user?id=123`, {
        method: 'POST',
        credentials: 'include' // CSRF risk
      }).then(() => {
        alert('User deleted!');
      });
    }
  }
};
</script>

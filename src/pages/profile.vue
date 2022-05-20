<template>
  <section class="section">
    <div class="container">


      <div class="columns">


        <div class="column">
          <h2 class="title">My Profile</h2>
          <div class="content">
            <p>
              <strong>Username:</strong>
              {{ loggedInUser.msg.user.name }}
            </p>
            <p>
              <strong>Email:</strong>
              {{ loggedInUser.msg.user.email }}
            </p>
          </div>
          <button class="button is-danger is-outlined">
            <span><FontAwesomeIcon :icon="['fas', 'right-from-bracket']"/> Logout</span>
          </button>
        </div>


        <div class="column">
          <h2 class="title">e-distribucion config</h2>
          <div class="content">
            <form class="box">
              <div class="field">
                <label class="label">Email</label>
                <div class="control">
                  <input class="input" type="email" placeholder="user@login.com" v-model="edis.username">
                </div>
              </div>

              <div class="field">
                <label class="label">Password</label>
                <div class="control">
                  <input class="input" type="password" v-model="edis.password">
                </div>
              </div>
              <div class="button is-primary" @click="send">
                <span><FontAwesomeIcon :icon="['fas', 'right-from-bracket']"/> Save and check</span>
              </div>
            </form>
            {{ loggedInUser }}
          </div>

        </div>

      </div>

    </div>


  </section>
</template>

<script>
import {mapGetters} from 'vuex'

export default {
  data: () => ({
    edis: {},
    error: [],
  }),
  middleware: ["auth"],
  computed: {
    ...mapGetters(['loggedInUser'])
  },
  methods: {
    async send() {
      try {
        this.$axios.$put('/edis/set/', this.edis)
          .then(response => {
            console.log(response);

            try {
              this.$axios.$get('/edis/me/')
                .then(response_second => {
                  console.log(response_second);
                  console.log("OK");
                });
            } catch (e_second){
                console.log(e_second)
            }

          });
      } catch (e) {
        this.error = [];
        console.log(response)
        e.response.data.msg.forEach(error => {
          this.error.push(error);
        });
      }
    }
  },
  mounted() {

    const {getters} = this.$store;

    let user = getters.loggedInUser;

    if (user.msg.user.edis) {
      this.edis = {
        username: "angel",
        password: "cardiel"
      }
    } else {
      this.edis = {
        username: "",
        password: ""
      }
    }


  }
}
</script>

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


              <div v-if="edis.username && !editing">

                <p>
                  <strong>Username:</strong>
                  <span v-if="loggedInUser.msg.user.edis">{{ loggedInUser.msg.user.edis.username }}</span>
                </p>
                <p>
                  <strong>Status:</strong>

                  <!-- Results -->
                  <span class="has-text-danger" v-if="status<0">Error de conexión</span>
                  <span class="has-text-success" v-if="status>0">Conexión válida</span>

                </p>


                <div class="button is-small is-light " @click="check"
                     v-bind:class="{ 'is-loading': loading }">
                  <span><FontAwesomeIcon :icon="['fas', 'arrows-rotate']"/> Check</span>
                </div>

                <div class="button is-small is-primary " v-bind:disabled="loading" @click="edit()">
                  <span><FontAwesomeIcon :icon="['fas', 'pen-to-square']"/> Edit</span>
                </div>

              </div>

              <template v-else>


                <div class="field">
                  <label class="label">Username</label>
                  <div class="control">
                    <input class="input" type="text" placeholder="user@login.com" v-model="edis.username">
                  </div>
                </div>


                <div class="field">
                  <label class="label">Password</label>
                  <div class="control">
                    <input class="input" type="password" v-model="edis.password">
                  </div>
                </div>


                <div class="columns">

                  <div class="column">
                    <div class="button is-light" @click="editing=false">
                      <span><FontAwesomeIcon :icon="['fas', 'ban']"/> Cancel</span>
                    </div>
                    <div class="button is-warning"
                         v-bind:class="{ 'is-loading': loading }"
                         @click="save">
                      <span><FontAwesomeIcon :icon="['fas', 'floppy-disk']"/> Save</span>
                    </div>

                  </div>


                </div>


              </template>


            </form>
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
    editing: false,
    status: 0,
    loading: false,
    response: false,
    valida: false,
  }),
  middleware: ["auth"],
  computed: {
    ...mapGetters(['loggedInUser'])
  },
  methods: {
    check() {
      this.loading = true
      this.status = 0
      this.$axios.$get('/edis/me/')
        .then(response => {
          this.loading = false
          this.status = 1
        }).catch(e => {
          this.loading = false
          this.status = -1
      })
    },
    edit() {
      this.editing = true
    },
    async save() {
        this.loading = true
        this.status = 0;
        this.$axios.$put('/edis/set/', this.edis)
          .then(response => {
            this.loading = false
            this.status = 0
            this.editing = false
          }).catch (e => {
            this.status = 0
            this.loading = false
        })
    }
  },
  mounted() {

    const {getters} = this.$store;

    let user = getters.loggedInUser;

    if (user.msg.user.edis) {
      this.edis = {
        username: user.msg.user.edis.username,
        password: ""
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

<template>
  <div>
    <div class="user-information" v-if="$auth.loggedIn">
      <div>{{ $auth.user.email }}</div>
      <button @click="$auth.logout()">Logout</button>
    </div>
    <div class="login-page">
      <div class="form">
        <form v-if="!isLogin" class="register-form" v-on:submit.prevent="registerUser">
          <input v-model="register.firstName" type="text" placeholder="first name" />
          <input v-model="register.lastName" type="text" placeholder="last name" />
          <input v-model="register.phone" type="text" placeholder="phone" />
          <input v-model="register.email" type="text" placeholder="email address" />
          <input v-model="register.password" type="password" placeholder="password" />
          <button>create</button>
          <p class="message">
            Already registered?
            <a @click.prevent="isLogin = true">Sign In</a>
          </p>
        </form>
        <form v-else class="login-form" v-on:submit.prevent="loginUser">
          <input v-model="phone" type="text" placeholder="phone" />
          <input v-model="password" type="password" placeholder="password" />
          <button type="submit">login</button>
          <p class="message">
            Not registered?
            <a @click.prevent="isLogin = false">Create an account</a>
          </p>
        </form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isLogin: true,
      register: {
        firstName: '',
        lastName: '',
        email: '',
        password: '',
        phone: '',
      },
      phone: '8123565698',
      password: '12345678',
    }
  },
  methods: {
    loginUser() {
      console.log('loggin in ...')
      this.$auth.loginWith('local', {
        data: {
          phone: this.phone,
          password: this.password,
        },
      })
    },
    async registerUser() {
      await this.$axios
        .post('/api/v1/auth/register', {
          first_name: this.register.firstName,
          last_name: this.register.lastName,
          phone: this.register.phone,
          email: this.register.email,
          password: this.register.password,
        })
        .catch((err) => {
          console.log(err)
        })

      this.$auth.loginWith('local', {
        data: {
          phone: this.register.phone,
          password: this.register.password,
        },
      })
    },
  },
}
</script>

<style>
@import url(https://fonts.googleapis.com/css?family=Roboto:300);

.login-page {
  width: 360px;
  padding: 8% 0 0;
  margin: auto;
}
.form {
  position: relative;
  z-index: 1;
  background: #ffffff;
  max-width: 360px;
  margin: 0 auto 100px;
  padding: 45px;
  text-align: center;
  box-shadow: 0 0 20px 0 rgba(0, 0, 0, 0.2), 0 5px 5px 0 rgba(0, 0, 0, 0.24);
}
.form input {
  font-family: 'Roboto', sans-serif;
  outline: 0;
  background: #f2f2f2;
  width: 100%;
  border: 0;
  margin: 0 0 15px;
  padding: 15px;
  box-sizing: border-box;
  font-size: 14px;
}
.form button {
  font-family: 'Roboto', sans-serif;
  text-transform: uppercase;
  outline: 0;
  background: #4caf50;
  width: 100%;
  border: 0;
  padding: 15px;
  color: #ffffff;
  font-size: 14px;
  -webkit-transition: all 0.3 ease;
  transition: all 0.3 ease;
  cursor: pointer;
}
.form button:hover,
.form button:active,
.form button:focus {
  background: #43a047;
}
.form .message {
  margin: 15px 0 0;
  color: #b3b3b3;
  font-size: 12px;
}
.form .message a {
  color: #4caf50;
  text-decoration: none;
}
.container {
  position: relative;
  z-index: 1;
  max-width: 300px;
  margin: 0 auto;
}
.container:before,
.container:after {
  content: '';
  display: block;
  clear: both;
}
.container .info {
  margin: 50px auto;
  text-align: center;
}
.container .info h1 {
  margin: 0 0 15px;
  padding: 0;
  font-size: 36px;
  font-weight: 300;
  color: #1a1a1a;
}
.container .info span {
  color: #4d4d4d;
  font-size: 12px;
}
.container .info span a {
  color: #000000;
  text-decoration: none;
}
.container .info span .fa {
  color: #ef3b3a;
}

.user-information {
  text-align: center;
  margin-top: 30px;
  font-size: 30px;
  padding: 5px;
  background-color: #f3f3f3;
}

body {
  background: #76b852; /* fallback for old browsers */
  background: -webkit-linear-gradient(right, #76b852, #8dc26f);
  background: -moz-linear-gradient(right, #76b852, #8dc26f);
  background: -o-linear-gradient(right, #76b852, #8dc26f);
  background: linear-gradient(to left, #76b852, #8dc26f);
  font-family: 'Roboto', sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>

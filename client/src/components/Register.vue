<template>
  <div class="wrapper">
    <h1>Регистрация</h1>
    <section class="auth">
      <b-field label="Имя">
        <b-input v-model="credentials.firstName" icon="account" placeholder="Иван"></b-input>
      </b-field>
      <b-field label="Фамилия">
        <b-input v-model="credentials.lastName" icon="account" placeholder="Иванов"></b-input>
      </b-field>
      <b-field label="Электронный адрес">
        <b-input
          type="email"
          v-model="credentials.email"
          icon="email"
          placeholder="test@example.com"
        ></b-input>
      </b-field>
      <b-field label="Пароль">
        <b-input
          type="password"
          v-model="credentials.password"
          password-reveal
          icon="lock"
          minlength="6"
          maxlength="100"
        ></b-input>
      </b-field>
      <b-field label="День рождения">
        <b-datepicker
          placeholder="Нажмите и выберите дату"
          icon="calendar-today"
          editable
          v-model="credentials.birth"
        ></b-datepicker>
      </b-field>
      <b-field label="Интересы">
        <b-input v-model="credentials.interests" icon="soccer" placeholder="Укажите через запятую"></b-input>
      </b-field>
      <div class="block">
        <label for class="label">Пол</label>
        <b-radio v-model="credentials.gender" native-value="male">Муж.</b-radio>
        <b-radio v-model="credentials.gender" native-value="female">Жен.</b-radio>
      </div>
    </section>
    <p v-if="error">{{ error }}</p>
    <a class="button is-primary" @click="tryRegister()">Зарегистрироваться</a>
    <p>
      <span class="action" @click="toLogin()">Уже есть аккаунт</span>
    </p>
  </div>
</template>

<script>
import AuthenticationService from '@/services/AuthenticationService'

export default {
  name: 'Register',
  data () {
    return {
      credentials: {
        email: '',
        password: '',
        lastName: '',
        firstName: '',
        interests: '',
        gender: '',
        birth: null
      },
      userData: null,
      error: null
    }
  },
  methods: {
    tryRegister () {
      AuthenticationService.register(this.credentials)
        .then(res => {
          console.log(res)
          this.$emit('register-success', res.data)
        })
        .catch(err => {
          if (err.response.status === 400) {
            this.error =
              'Пользователь с таким электронным адресом уже зарегистрирован.'
          }
        })
    },
    toLogin () {
      this.$emit('to-login')
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style>
label {
  text-align: left;
}
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>

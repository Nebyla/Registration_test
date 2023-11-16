<template>
  <div class="registration-form">
    <div class="accept" v-if="registrationSuccess">
      Регистрация прошла успешно
    </div>
    <div v-else class="card">
      <div class="card-header">
        <h1 class="card-title">Регистрация</h1>
      </div>
      <hr class="divider">
      <label class="switch">
        <input type="checkbox" v-model="showForm" />
        <span class="slider"></span>
      </label>
      <span class="tab"></span>
      <span class="profil">Публичный профиль</span>
      <div class="text-below">Включает профиль для просмотра другими пользователями по ссылке</div>
      <hr class="divider">
      <form class="card-body">
        <span class="profil">Данные</span>
        <div class="form-group">
          <input type="text" id="username" placeholder="Имя" v-model="username" class="form-control" required />
          <span class="tab"></span>
          <select id="jobPost" v-model="jobPost" class="form-сhoice" required>
            <option disabled value="">Должность</option>
            <option v-for="user in users" v-bind:key="user.value" :value="user.value">{{ user.names }}</option>
          </select>
        </div>
        <div class="form-group">
          <input type="email" id="email" placeholder="Email" v-model="email" class="form-control" required />
          <div class="error">{{ this.emailError }}</div>
        </div>
        <div class="form-group">
          <input type="text" id="password" placeholder="Пароль" v-model="password" class="form-control" required />
          <span class="tab"></span>
          <input type="text" id="password-repeat" placeholder="Повторите пароль" v-model="password_repeat"
            class="form-control" required />
            <div class="error">{{ this.passwordError }}</div>
        </div>
      </form>
      <hr class="divider">
      <label class="checkbox" @click="toggleCheck">
        <input type="checkbox" v-model="showCheck">
        <span class="checkmark"></span>
      </label>
      <span class="agreement">Нажимая на кнопку “Регистрация”, я подтверждаю свое согласение с политикой <br>
        <span class="tab-text"></span> конфиденциальности и обработки персональных данных</span>
      <div class="button-centre">
        <button type="submit" class="btn btn-primary" @click="fetchData" :disabled="!showCheck">Регистрация</button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios';
export default {
  props: {
    defaultValues: {
      type: Object,
      default: () => ({}),
    },
  },
  data() {
    return {
      username: this.defaultValues.username || '',
      email: this.defaultValues.email || '',
      password: this.defaultValues.password || '',
      password_repeat: this.defaultValues.password_repeat || '',
      showForm: true,
      showCheck: true,
      isEmailValid: true,
      emailError: '',
      passwordError: '',
      registrationSuccess: false,
      jobPost: '',
      users:
        [
          { value: 1, names: 'Бухгалтер' },
          { value: 2, names: 'Грузчик' },
          { value: 3, names: 'Программист' }
        ],
    };
  },
  methods: {
    fetchData: function () {
      var publicNumber = this.showForm ? 1 : 0
      var requestData = {
        public: publicNumber,
        username: this.username,
        role: this.jobPost,
        email: this.email,
        password: this.password,
        password_repeat: this.password_repeat,
      }

      console.log(this.jobPost)
      axios.post('https://lmstestapi.reezonly.com/v1/user/signup', requestData)
        .then((res) => {
          console.log(res.data)
          this.emailError = res.data.errors.email
          this.passwordError = res.data.errors.password_repeat
          this.registrationSuccess = res.data.success
        })
        .catch(function (res) {
          console.log(res.data)
        })

    },

  }
};
</script>

<style scoped>
.registration-form {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.divider {
  border-top: 1px solid #ccc;
  margin-top: 10px;
  margin-bottom: 10px;
}

.tab {
  padding-right: 2%
}

.tab-text {
  padding-right: 3.2%
}

.error{
  color: red;
}

.agreement {
  font-size: 14px;
  font: Montserrat;
  color: #696977;
}

.card {
  width: 958px;
  height: auto;
  padding: 20px;
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
  border-radius: 55px;
}

.card-title {
  font-size: 19px;
  font: Montserrat;
}
.accept{
  font-size: 36px;
  font: Montserrat;
  color: green;
}

.error-message {
  color: red;
  font-size: 12px;
  margin-top: 5px;
  display: block;
}

.has-error .form-control {
  border-color: red;
}

.profil {
  font-size: 16px;
  font: Montserrat;
}

.button-centre {
  text-align: center;
  margin-top: 20px;
}

.text-below {
  margin-bottom: 10px;
  margin-top: 8px;
  font-size: 14px;
  font: Montserrat;
  color: #696977;
}

.card-header {
  text-align: left;
}

.form-control {
  width: 450px;
  height: 39px;
  border: 1px solid #ced4da;
  border-radius: 5px;
  padding: 8px;
  margin-bottom: 15px;
}

.form-сhoice {
  width: 465px;
  height: 57px;
  border: 1px solid #ced4da;
  border-radius: 5px;
  padding: 8px;
  margin-bottom: 15px;
}

.form-group {
  margin-bottom: 15px;
  margin-top: 15px;
}


.btn-primary {
  margin-top: 20px;
  width: 234px;
  height: 39px;
  background-color: white;
  border: 1px solid #4CAF50;
  border-radius: 10px;
  color: #4CAF50;
}

.btn-primary:hover {
  background-color: #4CAF50;
  color: white;
}

.btn-primary:disabled {
  background-color: #ddd;
  border: 1px solid #ccc;
  color: #666;
  cursor: not-allowed;
}

.btn-primary:disabled:hover {
  background-color: #ddd;
  color: #666;
}

.checkbox {
  position: relative;
  padding-left: 30px;
  cursor: pointer;
  user-select: none;

}

.checkbox input {
  position: absolute;
  opacity: 0;
  cursor: pointer;
  height: 0;
  width: 0;
}

.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 15px;
  width: 15px;
  border: 1px solid black;
  display: inline-block;
  border-radius: 5px;
}

.checkmark:after {
  content: "";
  position: absolute;
  display: none;
  left: 4px;
  top: px;
  width: 5px;
  height: 10px;
  border: solid black;
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

.checkbox input:checked+.checkmark {
  background-color: #FFD467;
}

.checkbox input:checked+.checkmark:after {
  display: block;
}

.switch {
  margin-top: 10px;
  position: relative;
  display: inline-block;
  width: 39px;
  height: 19px;
}

.switch input {
  opacity: 0;
  width: 0;
  height: 0;
}

.slider {
  position: absolute;
  cursor: pointer;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #ccc;
  -webkit-transition: .4s;
  transition: .4s;
  border-radius: 19px;
}

.slider:before {
  position: absolute;
  content: "";
  height: 15px;
  width: 15px;
  left: 2px;
  bottom: 2px;
  background-color: white;
  -webkit-transition: .4s;
  transition: .4s;
  border-radius: 50%;
}

input:checked+.slider {
  background-color: #2196F3;
}

input:focus+.slider {
  box-shadow: 0 0 1px #2196F3;
}

input:checked+.slider:before {
  -webkit-transform: translateX(19px);
  -ms-transform: translateX(19px);
  transform: translateX(19px);
}
</style>

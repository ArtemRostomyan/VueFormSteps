<template>
  <div class="container mt-4">
    <div class="col-sm-4 mx-auto">

      <h2 class="reg-title">Регистрация</h2>
      <form @submit.prevent="userRegister" novalidate>
        <div v-show="step === 1">

          <div v-if="regMessage" class="alert alert-success" role="alert">
             Вы успешно зарегистрировались!
          </div>

          <div class="form-group">
              <label for="name">Ваше имя</label>

              <input @blur="$v.formReg.name.$touch()"
                     :class="status($v.formReg.name)"
                     v-model.trim="formReg.name"
                     type="text" class="form-control" id="name">

              <div class="invalid-feedback" v-if="!$v.formReg.name.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.name.alpha">{{ alphaText }}</div>
          </div>

          <div class="form-group">
              <label for="surname">Ваша фамилия</label>

              <input @blur="$v.formReg.surname.$touch()"
                     :class="status($v.formReg.surname)"
                     v-model.trim="formReg.surname"
                     type="text" class="form-control" id="surname">

              <div class="invalid-feedback" v-if="!$v.formReg.surname.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.surname.alpha">{{ alphaText }}</div>
          </div>

          <div class="form-group">
              <label for="email">Email</label>

              <input @blur="$v.formReg.email.$touch()"
                     :class="status($v.formReg.email)"
                     v-model.trim="formReg.email"
                     type="text" class="form-control" id="email">

              <div class="invalid-feedback" v-if="!$v.formReg.email.required">{{ reqText }}</div>
              <div class="invalid-feedback" v-if="!$v.formReg.email.email">Пожалуйста введите Email адрес</div>
          </div>

          <button @click="step++" :disabled="disabledBtn1"
                  type="button" class="btn btn-primary">Следующий шаг</button>

        </div>

        <transition name="slide-fade">
          <div v-show="step === 2">

              <div class="form-group">
                  <label for="country">Страна</label>

                  <input @blur="$v.formReg.country.$touch()"
                         :class="status($v.formReg.country)"
                         v-model.trim="formReg.country"
                         type="text" class="form-control" id="country">

                  <div class="invalid-feedback" v-if="!$v.formReg.country.alpha">{{ alphaText }}</div>
              </div>

              <div class="form-group">
                  <label for="city">Город</label>

                  <input @blur="$v.formReg.city.$touch()"
                         :class="status($v.formReg.city)"
                         v-model.trim="formReg.city"
                         type="text" class="form-control" id="city">

                  <div class="invalid-feedback" v-if="!$v.formReg.city.alpha">{{ alphaText }}</div>
              </div>

              <button @click="step--" type="button" class="btn btn-light mr-2">Назад</button>
              <button @click="step++" :disabled="disabledBtn2"
                      type="button" class="btn btn-primary">Следующий шаг</button>

          </div>
        </transition>

        <transition name="slide-fade">
          <div v-show="step === 3">

              <div class="form-group">
                  <label for="year">Год рождения</label>

                  <select @blur="$v.formReg.year.$touch()"
                          :class="status($v.formReg.year)"
                          v-model="formReg.year"
                          id="year" class="custom-select">
                      <option disabled value="">Выберите</option>
                      <option v-for="(year, index) in years" 
                              :value="year"
                              :key="index">{{ year }}</option>
                  </select>

                  <div class="invalid-feedback" v-if="!$v.formReg.year.required">{{ reqText }}</div>
              </div>

              <div class="form-group">
                  <label for="career">Профессия</label>
                  
                  <input @blur="$v.formReg.career.$touch()"
                         :class="status($v.formReg.career)"
                         v-model.trim="formReg.career"
                         type="text" class="form-control" 
                         id="career" placeholder="Например, Frontend">
                    
                  <div class="invalid-feedback" v-if="!$v.formReg.career.alpha">{{ alphaText }}</div>
              </div>

              <button @click="step--" type="button" class="btn btn-light mr-2">Назад</button>
              <button :disabled="disabledBtnFinish"
                      type="submit" class="btn btn-primary">Зарегистрироваться</button>

          </div>
        </transition>
      </form>

    </div>
  </div>
</template>

<script>
import { email, required, helpers } from 'vuelidate/lib/validators'

const alpha = helpers.regex('alpha', /^[a-zA-Zа-яёА-ЯЁ]*$/)

export default {
   data() {
     return {
        step: 1,
        regMessage: false,
        years: [],
        yearEnd: 2020,
        reqText: 'Поле обязательно для заполнения',
        alphaText: 'Запрещены цифры, пробелы и другие символы',
        formReg: {
          email: '',
          name: '',
          surname: '',
          country: '',
          city: '',
          year: '',
          career: ''
        }
     }
   },
   computed: {
    disabledBtn1() {
       return this.$v.formReg.name.$invalid || this.$v.formReg.surname.$invalid || this.$v.formReg.email.$invalid
    },
    disabledBtn2() {
       return this.$v.formReg.country.$invalid || this.$v.formReg.city.$invalid
    },
    disabledBtnFinish() {
       return this.$v.formReg.year.$invalid || this.$v.formReg.career.$invalid
    }
   },
   methods: {
    status(validation) {
       return {
         'is-invalid': validation.$error,
         'error': validation.$error
       }
    },
    userRegister() {
        console.group()
          console.log('Вы успешно зарегистрированны!')
          console.log('Ваше имя: ' + this.formReg.name)
          console.log('Ваша фамилия: ' + this.formReg.surname)
          console.log('Email: ' + this.formReg.email)
          console.log('Страна: ' + this.formReg.country)
          console.log('Город: ' + this.formReg.city)
          console.log('Год рождения: ' + this.formReg.year)
          console.log('Профессия: ' + this.formReg.career)
        console.groupEnd()

        this.reset()
        
    },
    reset() {
        // сбросить шаг и показать сообщение о регистрации
        this.step = 1;
        this.regMessage = true;

        // убрать сообщение о регистрации
        setTimeout(() => {
          this.regMessage = false
        }, 3000)

        // сбросить все поля
        for (let input in this.formReg) {
            this.formReg[input] = ''
        }

        // сбросить валидацию
        this.$v.$reset()
    }
  },
  validations: {
      formReg: {
          email: {
            email,
            required
          },
          name: {
            required,
            alpha
          },
          surname: {
            required,
            alpha
          },
          country: {
            alpha
          },
          city: {
            alpha
          },
          year: {
            required
          },
          career: {
            alpha
          }
      }
  },
  created() {
      for (let i = this.yearEnd; i >= 1800; i--) this.years.push(i)
  }
}
</script>

<style>
body {
  background-color: #f1f1f1;
}

form {
  background-color: white;
  padding: 20px;
  border-radius: 10px;
  box-shadow: 10px 10px 45px -31px rgba(0, 0, 0, 0.75);
}

.error {
  background-color: #fdd;
}

.reg-title {
  color: #5d5d5d;
  font-size: 24px;
  margin-bottom: 18px;
  padding-left: 20px;
}

.slide-fade-enter-active {
  transition: all 0.3s ease;
}

.slide-fade-enter {
  transform: translateX(10px);
  opacity: 0;
}
</style>

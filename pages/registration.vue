<template>
  <section class="container w-50  py-5">
    <b-form @submit.prevent="sendData">
          <div>
            <div class="d-flex w-100 justify-content-between">
              <b-form-group class="first_name" id="input-group-2" label="First Name:" label-for="input-2">
                <b-form-input
                  id="input-2"
                  v-model="form.first_name"
                  :state="isValidField('first_name')"
                  placeholder="First name"
                ></b-form-input>
              </b-form-group>

              <b-form-group class="last_name" id="input-group-2" label="Last Name:" label-for="input-2">
                <b-form-input
                  id="input-2"
                  v-model="form.last_name"
                    :state="isValidField('last_name')"
                  placeholder="Last name"
                ></b-form-input>
              </b-form-group>
            </div>
          </div>

          <b-form-group
            id="input-group-1"
            label="Email:"
            label-for="input-1">
            <b-form-input
              id="input-1"
              v-model="form.email"
                :state="isValidField('email')"
              type="text"
              placeholder="Email"
            ></b-form-input>
          </b-form-group>

         <b-form-group
           id="input-group-1"
           label="Password:"
           label-for="input-1">
           <b-form-input
             id="input-1"
             v-model="form.password"
               :state="isValidField('password')"
             type="password"
             placeholder="Password"
           ></b-form-input>
         </b-form-group>

         <b-form-group
           id="input-group-1"
           label="Repeat Password:"

           label-for="input-1">
           <b-form-input
             id="input-1"
               :state="isValidField('repeat_password')"
             v-model="form.repeat_password"
             type="password"
             placeholder="Repeat Password"
           ></b-form-input>
         </b-form-group>

         <b-button class="d-block ml-auto" type="submit" variant="primary">Registrate Me</b-button>
       </b-form>
  </section>
</template>

<script>
import Logo from '~/components/Logo.vue'
import {required, minLength, email, sameAs} from 'vuelidate/lib/validators';

export default {
  layout: 'frontend',
  components: {
    Logo
  },
  data() {
    return {
        wasSumitted: false,
        form: {
          first_name: '',
          last_name: '',
          email: '',
          password: '',
          repeat_password: ''
        }
    }
  },
  methods: {
    async sendData() {
      this.wasSumitted = true;
      this.$v.$touch();

      if (!this.$v.form.$invalid) {

      }
    },
    isValidField(field) {
      const rez = !this.$v.form[field].$invalid;
      return this.wasSumitted ? rez : null;
    },
  },
  validations: {
    form: {
      first_name: {
          required,
          minLength: minLength(3)
      },
      last_name: {
        required,
        minLength: minLength(3)
      },
      email: {
          required,
          email,
          async isUnique(val) {
              let rez = false;

              console.log('check email');
              rez = await new Promise((res, rej) => {
                setTimeout(() => {
                       res(true);
                   }, 300);
              });

              return rez;
          }
      },
      password: {
        required,
        minLength: minLength(6)
      },
      repeat_password: {
        required,
        sameAsPassword: sameAs('password')
      }
    }
  }
}
</script>

<style scoped lang="scss">
  .first_name,
  .last_name {
    width:45%;
  }
</style>

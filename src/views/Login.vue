<template>
  <div class="loginBox">
    <div class="box">
      <v-form ref="form" v-model="valid" lazy-validation>
        <v-text-field
          v-model="name"
          name="name"
          :rules="nameRules"
          :counter="10"
          label="Name"
          required
        />
        <v-text-field
          v-model="password"
          name="password"
          :append-icon="pwShow ? 'visibility_off' : 'visibility'"
          :rules="[passwordRules.required, passwordRules.min]"
          :type="pwShow ? 'text' : 'password'"
          label="Password"
          hint="At least 8 characters"
          autocomplete="password"
          counter
          @click:append="pwShow = !pwShow"
        />
        <div class="submitBtn">
          <button
            type="button"
            @click="submit()"
            class="v-btn"
            name="submit"
          >
            <div class="v-btn__content">
              submit
            </div>
          </button>
          <v-btn
            @click="clear"
            name="clear"
          >
            clear
          </v-btn>
        </div>
      </v-form>
    </div>
  </div>
</template>

<script>
import auth from '@/auth';
import * as VForm from 'vuetify/es5/components/VForm';
import * as VTextField from 'vuetify/es5/components/VTextField';

export default {
  name: 'Login',
  components: {
    ...VForm,
    ...VTextField
  },
  data () {
    return {
      valid: true,
      name: '',
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 10) || 'Name must be less than 10 characters'
      ],
      pwShow: false,
      password: '',
      passwordRules: {
        required: value => !!value || 'Required.',
        min: v => v.length >= 8 || 'Min 8 characters'
      }
    };
  },
  methods: {
    submit () {
      if (this.$refs.form.validate()) {
        auth.login({
          id: this.name,
          password: this.password
        }).then((data) => {
          this.$router.replace(this.$route.query.redirect || '/list');
        }).catch((error) => {
          alert('로그인 에러');
          /*
           * TODO : ERROR 처리는 어떻게 표현 할지 고민.
           *   - 알림 모듈 생성하여 관리?
           * */
        });
      } else {
        alert('id. pw 입력');
      }
    },
    clear () {
      // this.$refs.form.reset();
      this.name = '';
      this.password = '';
    }
  }
};
</script>

<style lang="less">
  @import '../less/mixin';
  .loginBox {
    position:absolute;
    top:50%;
    left:0;
    margin-top:-112px;
    padding:0 20px;
    width:100%;
    .box {
      margin:0 auto;
      padding:20px;
      width:100%;
      max-width:500px;
      background-color:#464545;
      box-shadow: 0 5px 15px rgba(0, 0, 0, 0.4);
      .submitBtn {
        text-align:center;
      }
    }
  }
</style>




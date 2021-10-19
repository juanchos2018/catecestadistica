<!-- 
	This is the sign in page, it uses the dashboard layout in: 
	"./layouts/Default.vue" .
 -->

<template>
  <div class="sign-in">
    <a-row type="flex" :gutter="[24, 24]" justify="space-around" align="middle">
      <!-- Sign In Form Column -->
      <a-col
        :span="24"
        :md="12"
        :lg="{ span: 12, offset: 0 }"
        :xl="{ span: 6, offset: 2 }"
        class="col-form"
      >
        <h1 class="mb-15">Bienvenido</h1>
        <h5 class="font-regular text-muted">Ingresar Usuario y constraseña</h5>

        <!-- Sign In Form -->
        <a-form
          id="components-form-demo-normal-login"
          :form="form"
          class="login-form"
          @submit.prevent="Login"
        >
          <a-form-item class="mb-10" label="Usuario" :colon="false">
            <a-input  v-model="email" placeholder="Usuario" />
          </a-form-item>
          <a-form-item class="mb-5" label="Contraseña" :colon="false">
            <a-input type="password" v-model="password" placeholder="Password" />
          </a-form-item>          
          <a-form-item>
            <a-button
              type="primary"
              block
              html-type="submit"
              class="login-form-button"
            >
              INGRESAR
            </a-button>
          </a-form-item>
        </a-form>
      </a-col>

      <a-col :span="24" :md="10" :lg="10" :xl="10">
        <img src="images/catec.jpg" width="310" height="430" alt="" />
             <!-- <img src="images/redjuvenil.png" width="270" height="300" alt="" /> -->
      </a-col>
      <!-- Sign In Image Column -->
    </a-row>
  </div>
</template>

<script>
import axios from "axios";
const Swal = require("sweetalert2");
import { mapState } from 'vuex'

export default {
  data() {
    return {
      // Binded model property for "Sign In Form" switch button for "Remember Me" .
      rememberMe: true,
      USER_ADMIN:'admin',
      PASSOWRD_ADMIN:'123456',
	    email: '',
      password: '',
      UserModel: {
        correo: "",
        clave: "",
      },
    };
  },
  beforeCreate() {
    // Creates the form and adds to it component's "form" property.
    this.form = this.$form.createForm(this, { name: "normal_login" });
  },
  computed: {
    ...mapState(['url_base'])
   
  },
  methods: {
    Login() {    
      if (this.email.length == 0) {
        this.errors.email = true;
      }
      if (this.password.length == 0) {
        this.errors.password = true;
      }

      let me = this;
      const email = this.email;
      const password = this.password;
	  //  let url =  "loginUser";

    if (me.USER_ADMIN==email && me.PASSOWRD_ADMIN==password) {
            me.$session.start()
         //   localStorage.setItem("tipousuario", response.data.result.tipousuario )
           // localStorage.setItem("itipo_tipousuario", response.data.result.id_tipousuario )
            me.$router.push("/");
      }else{
         Swal.fire({
            icon: "error",
            title: "Datos incorrectos",
            showConfirmButton: false,
            timer: 1500,
          });
      }
     
    },  
    handleSubmit(e) {
      e.preventDefault();
      this.form.validateFields((err, values) => {
        if (!err) {
          console.log("login ", values);
          let me = this;
          me.UserModel.correo = value.email;
          me.UserModel.correo = value.password;
          const data = me.UserModel;
          let url = "loginUser";
          axios({
            method: "POST",
            url: url,
            data: data,
          })
            .then(function(response) {
              console.log(response);

              alert("dfdf");
              if (response.data.status == "200") {
                //	me.MensajeOk("Registrado con Exito !")
              } else if ((response.data.status = "404")) {
                Swal.fire({
                  icon: "warning",
                  text: response.data.response,
                  timer: 3000,
                });
              }
            })
            .catch((error) => {
              console.log(error);
            });
          //   this.$router.push("/");
        }
      });
    },
  },
};
</script>

<style lang="scss">
body {
  background-color: #ffffff;
}
</style>

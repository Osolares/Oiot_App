<template>
  <div class="container login-page">
    <div class="col-lg-4 col-md-6 ml-auto mr-auto">
      <card class="card-login card-white">
        <template slot="header">
          <img src="img//card-primary.png" alt="" />
          <h1 class="card-title">OIoT</h1>
        </template>

        <div>
          <base-input
            name="name"
            v-model="user.name"
            placeholder="Name"
            addon-left-icon="tim-icons icon-badge"
          >
          </base-input>

          <base-input
            name="email"
            v-model="user.email"
            placeholder="Email"
            addon-left-icon="tim-icons icon-email-85"
          >
          </base-input>

          <base-input
            name="password"
            v-model="user.password"
            type="password"
            placeholder="Password"
            addon-left-icon="tim-icons icon-lock-circle"
          >
          </base-input>
        </div>

        <div slot="footer">
          <base-button
            native-type="submit"
            type="primary"
            class="mb-3"
            size="lg"
            @click="register()"
            block
          >
            Register
          </base-button>

          <div class="pull-left">
            <h6>
              <nuxt-link class="link footer-link" to="/login">
                Login
              </nuxt-link>
            </h6>
          </div>

          <div class="pull-right">
            <h6><a href="#help!!!" class="link footer-link">Necesitas Ayuda?</a></h6>
          </div>
        </div>
      </card>
    </div>
  </div>
</template>
<script>
import Swal from 'sweetalert2';
import 'sweetalert2/dist/sweetalert2.css'

export default {
  middleware: 'notAuthenticated',
  layout: "auth",
  data() {
    return {
      user: {
        name: "",
        email: "",
        password: ""
      }
    };
  },
  methods: {
    register() {

      this.$axios
        .post("/register", this.user)
        .then(res => {
          //success! - Usuario creado.
          if (res.data.status == "success") {


    // Mostrar mensaje de confirmación con SweetAlert2
    const result = Swal.fire({
      title: 'EXITO',
      text: "El usuario a sido creado exitosamente ¿Desea Ingresar al Dashboard?",
      icon: 'success',
      showCancelButton: true,
      confirmButtonColor: '#d33',
      cancelButtonColor: '#3085d6',
      confirmButtonText: 'Si',
      cancelButtonText: 'No'
    });

    // Si el usuario confirma la eliminación
    if (result.isConfirmed) {
      window.location.href = '/login'; // Redirige a la página de login


    } else {


    }

            


            this.$notify({
              type: "success",
              icon: "tim-icons icon-check-2",
              message: "Success! Now you can login..."
            });

            this.user.name = "";
            this.user.password = "";
            this.user.email = "";

            return;
          }

        })
        .catch(e => {
          console.log(e.response.data);

          if (e.response.data.error.errors.email.kind == "unique") {
            // Mostrar alerta de error
             Swal.fire({
              icon: 'error',
              title: 'Error',
              text: 'Error el usuario ya existe, intente con otros datos',
            });
            
            this.$notify({
              type: "danger",
              icon: "tim-icons icon-alert-circle-exc",
              message: "User already exists :("
            });

            return;

          } else {
            // Mostrar alerta de error
             Swal.fire({
              icon: 'error',
              title: 'Error',
              text: 'Error creando usuario, intente nuevamente ',
            });

            this.$notify({
              type: "danger",
              icon: "tim-icons icon-alert-circle-exc",
              message: "Error creating user..."
            });

            return;
          }



        });


    }
  }
};
</script>
<style>
.navbar-nav .nav-item p {
  line-height: inherit;
  margin-left: 5px;
}
</style>

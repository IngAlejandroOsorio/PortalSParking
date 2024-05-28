<template>
	<div>
		<div class="page-wrapper">
			<div class="container-fluid p-0">
				<!-- login page with video background start-->
				<div class="auth-bg-video">
					<video id="bgvid" poster="../assets/images/other-images/coming-soon-bg.jpg" playsinline="" autoplay="" muted="" loop="">
						<source src="../assets/video/auth-bg.mp4" type="video/mp4">
						</video>
						<div class="authentication-box">
							<div class="text-center"><img src="../assets/images/endless-logo.png" alt=""></div>
							<div class="card mt-4">
								<div class="card-body">
									<div class="text-center">
										<h4>Ingreso a SmartParking</h4>
										<h6>Coloca tu correo y contraseña para continuar...</h6>
									</div>
                                    <form @submit.prevent="loginUser" class="theme-form">
										<div class="form-group">
											<label class="col-form-label pt-0">Correo</label>
                                            <input v-model="email" class="form-control" id="email" type="email" required />
										</div>
										<div class="form-group">
											<label class="col-form-label">Contraseña</label>
                                            <input v-model="password" autocomplete="" class="form-control" id="password" type="password" required />											
										</div>
										<div class="checkbox p-0">
											<input id="checkbox1" type="checkbox">
											<label for="checkbox1">Recordarme</label>
										</div>
										<div class="form-group form-row mt-3 mb-0">
                                            <button type="submit" class="btn btn-primary btn-block" >Ingresar</button>
										</div>										
									</form>
								</div>
							</div>
						</div>
					</div>
					<!-- login page with video background end-->
				</div>
			</div>
		</div>
	</template>
<script>
import { getAuth, signInWithEmailAndPassword } from "firebase/auth";

export default {
  data() {
    return {
      email: "",
      password: ""
    };
  },
  methods: {
    async loginUser() {
      const auth = getAuth();
      try {
        //localStorage.setItem('user',this.email);
        await signInWithEmailAndPassword(auth, this.email, this.password);        
        this.$router.push('/');
      } catch (error) {
        this.$toasted.show('Oops...' + err.message, {theme: 'bubble',   position: "bottom-right",   type: 'error', duration: 2000});        
      }
    }
  }
};
</script>
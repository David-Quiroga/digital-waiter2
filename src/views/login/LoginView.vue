<template>
    <body>
        <h1>HOLA</h1>
        <h1 class="titulo1">DE NUEVO!</h1>
        <p class="comm">"La creacion es la expresion de la vida"</p>
        <p class="autor">-Rollo May</p>
            <div class="form-wrapper">
                <h2>Inicio de Sesion</h2>
            <form @submit.prevent="iniciarSesion">
                <div class="form-control">
                    <input type="text" v-model="correoElectronico" required>
                    <label>Correo Electronico</label>
                </div>
                <div class="form-control">
                    <input type="password" v-model="password" required>
                    <label>Password</label>
                </div>
                <button type="submit">Sign me In</button>
                <div class="form-help">
                    <router-link to="/registro">
                        <a>Aun no tienes cuenta?</a>
                    </router-link>
                </div>
            </form>
        </div>
    </body>
</template>

<script>
    import axios from 'axios';
    export default {
        name: 'LoginView',
            data() {
            return {
            correoElectronico: '',
            password: ''
        };
    },
    methods: {
        async iniciarSesion() {
            try {
            const response = await axios.post('http://localhost:4200/usuario/login', {
                correoElectronico: this.correoElectronico,
                password: this.password
            });
            const { usuario, token } = response.data;
                localStorage.setItem("user", JSON.stringify(usuario));
                localStorage.setItem("token", token);
                console.log(response.data);
                // Redireccionar al dashboard o a la vista correspondiente
                this.$router.push('/restaurante');
            } catch (error) {
                console.error('Error al iniciar sesión:', error.response ? error.response.data : error.message);
            }
        }
    }
};
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playwrite+CU:wght@100..400&display=swap');
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: 'Roboto', sans-serif;
}
body {
    background: #000;
}
body::before {
    content: "";
    position: absolute;
    left: 0;
    top: 0;
    width: 100%;
    height: 100%;
    background: url("../../assets/img/login.jpg");
    background-position: center;
    background-size: cover;
}
h1 {
    position: absolute;
    top: 100px;
    left: 40px;
    color: #fff;
    font-size: 90px;
}
.comm {
    position: absolute;
    top: 470px;
    left: 40px;
    color: #fff;
    font-size: 25px;
    font-family: "Playwrite CU", cursive;
    font-optical-sizing: auto;
    font-style: normal;
}
.autor {
    position: absolute;
    top: 540px;
    left: 540px;
    color: #fff;
    font-size: 20px;
    font-family: "Playwrite CU", cursive;
    font-optical-sizing: auto;
    font-style: normal;
}
.titulo1 {
    position: absolute;
    top: 340px;
    left: 40px;
    color: #fff;
    font-size: 90px;
}
.form-wrapper {
    position: absolute;
    left: 81%;
    top: 30%;
    width: 587px;
    height: 1115px;
    border-radius: 4px;
    padding: 70px;
    transform: translate(-50%, -50%);
    background: rgba(0, 0, 0, .75);
}
.form-wrapper h2 {
    color: #fff;
    top: 450px;
    position: absolute;
    font-size: 2rem;
}
.form-wrapper form {
    margin: 500px 0 65px;
}
form .form-control {
    height: 50px;
    position: relative;
    margin-bottom: 16px;
}
.form-control input {
    height: 100%;
    top: -30px;
    position: absolute;
    width: 100%;
    background: #333;
    border: none;
    outline: none;
    border-radius: 4px;
    color: #fff;
    font-size: 1rem;
    padding: 0 20px;
}
.form-control input:is(:focus, :valid) {
    background: #444;
    padding: 16px 20px 0;
}
.form-control label {
    position: absolute;
    left: 20px;
    top: -10%;
    transform: translateY(-50%);
    font-size: 1rem;
    pointer-events: none;
    color: #8c8c8c;
    transition: all 0.1s ease;
}
.form-control input:is(:focus, :valid)~label {
    font-size: 0.75rem;
    transform: translateY(-130%);
}
form button {
    width: 100%;
    padding: 16px 0;
    font-size: 1rem;
    background: #FF7A00;
    color: #fff;
    font-weight: 500;
    border-radius: 4px;
    border: none;
    outline: none;
    margin: 25px 0 10px;
    cursor: pointer;
    transition: 0.1s ease;
}
form button:hover {
    background: #f78821;
}
.form-wrapper a {
    text-decoration: none;
}
.form-wrapper a:hover {
    text-decoration: underline;
}
.form-wrapper :where(label, p, small, a) {
    color: #b3b3b3;
}
form .form-help {
    display: flex;
    justify-content: space-between;
}
form .remember-me {
    position: absolute;
    top: 870px;
    display: flex;
}
form .remember-me input {
    margin-right: 5px;
    accent-color: #b3b3b3;
}
form .form-help :where(label, a) {
    font-size: 0.9rem;
}
.form-wrapper p a {
    color: #fff;
}
.form-wrapper small {
    display: block;
    margin-top: 15px;
    color: #b3b3b3;
}
.form-wrapper small a {
    color: #0071eb;
}

/* Media queries for screens with a max-width of 720px */
@media (max-width: 720px) {
    h1 {
        font-size: 48px;
        top: 50px;
        left: 170px;
    }
    .titulo1 {
        font-size: 48px;
        top: 50px;
        left: 310px;
    }
    .comm {
        font-size: 18px;
        top: 550px;
        left: 150px;
    }
    .autor {
        font-size: 16px;
        top: 590px;
        left: 170px;
    }
    .form-wrapper {
        width: 60%;
        height: auto;
        left: 50%;
        top: 50%;
        padding: 40px;
        transform: translate(-50%, -50%);
    }
    .form-wrapper h2 {
        top: auto;
        position: relative;
        font-size: 1.5rem;
        margin-bottom: 30px;
    }
    .form-wrapper form {
        margin: 0;
    }
    .form-control {
        margin-bottom: 12px;
    }
    .form-control input {
        font-size: 0.875rem;
    }
    form button {
        padding: 12px 0;
        font-size: 0.875rem;
    }
    form .remember-me {
        position: static;
        margin-top: 10px;
    }
}
</style>

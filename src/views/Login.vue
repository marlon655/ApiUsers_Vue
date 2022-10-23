<template>
    <div>
        <h2>Area de Login!</h2>
        <hr>
        <div class="columns is-mobile is-centered">
        <div class="column is-half">
            <div v-if="error != undefined">
                <div class="notification is-danger is-light">
                    <strong>{{error}}</strong>
                </div>      
            </div>
            <p>Email:</p>
            <input type="email" placeholder="email@email.com" class="input" v-model="email">
            <p>Senha:</p>
            <input type="password" placeholder="******" class="input" v-model="password">
          
            <button id="Bcadastro" class="button is-primary" @click="login">Logar!</button>
        </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
    export default{
        data(){
            return{
               password:'',
               email:'',
               error: undefined
            }
        },
        methods:{
            login(){
            axios.post("http://localhost:8686/login",{
                password: this.password,
                email: this.email
            }).then(res => {
                console.log(res);
                // console.log(res.data.token);
                localStorage.setItem('token', res.data.token);
            }).catch(err => {
                var msgErro = err.response.data.err;
                this.error = msgErro;
            })
        }
    }
}
</script>
<style scoped>
    #Bcadastro{
        margin-top: 10px;
    }
    .columns.is-mobile{
        width: 100%;
    }

</style>
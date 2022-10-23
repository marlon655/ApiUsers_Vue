<template>
    <div>
        <h2>Registro de Usuário!</h2>
        <hr>
        <div class="columns is-mobile is-centered">
        <div class="column is-half">
            <div v-if="error != undefined">
                <div class="notification is-danger is-light">
                    <strong>{{error}}</strong>
                </div>      
            </div>
            <p>Nome:</p>
            <input type="text" placeholder="Nome do usuário" class="input" v-model="name">
            <p>Email:</p>
            <input type="email" placeholder="email@email.com" class="input" v-model="email">
            <p>Senha:</p>
            <input type="password" placeholder="******" class="input" v-model="password">
          
            <button id="Bcadastro" class="button is-primary" @click="register">Cadastrar!</button>
        </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
    export default{
        data(){
            return{
               name:'',
               password:'',
               email:'',
               error: undefined
            }
        },
        methods:{
            register(){
            axios.post("http://localhost:8686/user",{
                name: this.name,
                password: this.password,
                email: this.email
            }).then(res => {
                this.$router.push({name:'home'});
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
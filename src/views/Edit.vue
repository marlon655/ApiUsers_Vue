<template>
    <div>
        <h2>Edição de Usuário!</h2>
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
          
            <button id="Bcadastro" class="button is-primary" @click="update">Editar!</button>
        </div>
        </div>
    </div>
</template>
<script>
import axios from 'axios';
    export default{
        created(){
            var config = {
                headers: {
                    'Authorization': "bearer " + localStorage.getItem('token')
                }
            }

            axios.get("http://localhost:8686/user/" + this.$route.params.id, config).then(res => {
                console.log(res);
                this.name = res.data.name;
                this.email = res.data.email;
                this.id = res.data.id;
            }).catch(err => {
                console.log(err.response);
                this.$router.push({name: 'Users'});
            })
        },
        data(){
            return{
               name:'',
               email:'',
               id: -1,
               error: undefined
            }
        },
        methods:{
            update(){
            var config = {
                headers: {
                    'Authorization': "bearer " + localStorage.getItem('token')
                }
            }

            axios.put("http://localhost:8686/user",{
                name: this.name,
                email: this.email,
                id: this.id
            }, config).then(res => {
                this.$router.push({name:'Users'});
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
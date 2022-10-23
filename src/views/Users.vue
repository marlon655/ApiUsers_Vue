<template>
    <div>
        <h1>Painel Adm!</h1>

        <table class="table">
                <thead>
                    <tr>
                    <th>Nome</th>
                    <th>Email</th>
                    <th>Cargo</th>
                    <th>Ações</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="user in users" :key="user.id">
                        <td>{{user.name}}</td>
                        <td>{{user.email}}</td>
                        <td>{{Role(user.role)}}</td>
                        <td>
                            <router-link :to="{name: 'UserEdit', params:{id: user.id}}"><button class="button is-warning espaco ">Editar</button></router-link>
                            <button class="button is-danger" @click="showModalUser(user.id)">Deletar</button>
                        </td>
                    </tr>
                </tbody>
            </table>
            
            <div :class="{modal: true, 'is-active': showModal}">
                <div class="modal-background"></div>
                <div class="modal-content">
                    
                    <div class="card">
                        <div class="card-content">
                            <div class="content">
                                <p>Deseja excluir o item selecionado?</p>
                            </div>
                        </div>
                        <footer class="card-footer">
                            <a href="#" class="card-footer-item" @click="hideModal()">No</a>
                            <a href="#" class="card-footer-item" @click="deleteUser()">Yes</a>
                        </footer>
                        </div>

                </div>
                    <button class="modal-close is-large" aria-label="close" @click="hideModal()"></button>
            </div>

    </div>
</template>

<script>
import axios from 'axios'
export default {
    created(){
        var config = {
            headers: {
                'Authorization': "bearer " + localStorage.getItem('token')
            }
        }
        axios.get("http://localhost:8686/user",config).then(res => {
            
            this.users = res.data;
            // console.log(res.data);
        }).catch(err => {
            console.log(err);
        })
    },
    data(){
        return{
            users:[],
            showModal: false,
            deleteUserId: -1
        }
    },
    methods:{
        Role: function(value){
            if(value == 0){
                return "Comum";
            }else if(value == 1){
                return "Admin";
            }
        },

        hideModal(){
            this.showModal = false;
        },
        showModalUser(id){
            //console.log("id do usuário:" + id);
            this.deleteUserId = id;
            this.showModal = true;
        },
        deleteUser(){
            var config = {
                headers: {
                    'Authorization': "bearer " + localStorage.getItem('token')
                }
            }

            axios.delete("http://localhost:8686/user/"+ this.deleteUserId, config).then(res => {
                console.log(res);
                this.showModal = false;
                this.users = this.users.filter(u => u.id != this.deleteUserId);
            }).catch(err => {
                console.log(err);
                this.showModal = false;
            });
        }
    }
}
</script>

<style scoped>
.espaco{
    margin-right: 10px;
}
</style>
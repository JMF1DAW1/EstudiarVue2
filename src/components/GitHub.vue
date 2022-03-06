<template>
    <div>
        <!-- TODO: Crear componente GitHub -->
        <div>
            <input type="text" placeholder="Introduce un usuario" v-model="user" @keydown.enter="obtenerUsuario">
        </div>

        <div v-if="userValid">
             {{ userData.login }}
            <img :src="userData.avatar_url" style="width: 20%">
            <br>
            <button type="button" @click="obtenerRepositorios"> Repositorios </button> 
            <a :href="userData.html_url" target="_blank"> Ir a URL </a>
        </div>

        <!--<div v-if="mostrarRepo">
            <div v-for="(item, id) in repositorios" :key="id">
                {{ item.name }} -- {{ item.forks_count}} <img src="imagen_fork.png" style="width:8px">
                -- Enlace: <a :href="item.html_url" target="_self">  {{ item.name }}</a>
            </div>
        </div>-->
        <div v-if="mostrarRepo">
           <GitHubRepos :repolist="repositorios"></GitHubRepos>
        </div>
    </div>
</template>

<script>

// TODO: Importar componente GitHubRepos
import GitHubRepos from "./GitHubRepos.vue"

export default {
    name: 'GitHub',
    components: {
        // TODO: Importar componente GitHubRepos
        GitHubRepos
    },
    data: function() {
        return {
            // TODO: crear variables de datos para el funcionamiento del componente
            user: "",
            userData: {}, 
            userValid: false,
            repositorios: {},
            mostrarRepo: false,
        }
    },
    methods: {
        obtenerUsuario: function() {
            var userAuth = process.env.VUE_APP_USERNAME || "user";
            var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

            let headers = new Headers();
            headers.set('Authorization', 'Basic ' + btoa(userAuth + ":" + passAuth));

            var url = "https://api.github.com/users/" + this.user;

            fetch(url, {method:'GET',
                    headers: headers,
                })
            .then(res => res.json())
            .then(response => {
                console.log(response);
                this.userData = response;
                this.userValid = true;
            })
            .catch(error => {
                console.log(error);
            })
        },
        obtenerRepositorios: function() {
            var userAuth = process.env.VUE_APP_USERNAME || "user";
            var passAuth = process.env.VUE_APP_USERTOKEN || "pass";

            let headers = new Headers();
            headers.set('Authorization', 'Basic ' + btoa(userAuth + ":" + passAuth));

            var url = this.userData.repos_url

            fetch(url, {method:'GET',
                    headers: headers,
                })
            .then(res => res.json())
            .then(response => {
                console.log(response);
                this.repositorios = response;
                this.mostrarRepo = true;
            })
            .catch(error => {
                console.log(error);
            })

        }
    }
}
</script>

<style scoped>
</style>

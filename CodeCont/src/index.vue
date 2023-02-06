
<script setup>
// import HelloWorld from './components/HelloWorld.vue'
import './assets/index.css';
import axios from "axios";
import 'bootstrap/dist/css/bootstrap.min.css';
</script>

<template>

    <head>
        <!-- <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.4.1/dist/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">    -->
    </head>

    <header>
        <h1>TO-DO</h1>
        <section>
            <button class="btn btn-outline-light" @click="perfil">cambio</button>
            <h3>{{ usuario.nombre }} {{ usuario.apellido }}</h3>
            <img :src="usuario.perfil" :alt="usuario.nombre">
        </section>
    </header>

    <main class="container">
        <article class="input-group">
            <input type="text" class="form-control" v-model="evento.texto" @keydown="generador">
            <input type="datetime-local" class="form-control" v-model="evento.fecha">
            &nbsp;&nbsp;&nbsp;&nbsp;
            <button class="btn btn-outline-light" @click="crear">Crear</button>
        </article>
        <ol v-for="miEvento, i in lista">
            <li>
                <div >
                    <p class="elUsuario">
                        <img :src="miEvento.usuario.perfil" :alt="miEvento.usuario.nombre">
                        &nbsp;&nbsp;&nbsp;&nbsp;
                        <h3>{{ miEvento.usuario.nombre }} {{ miEvento.usuario.apellido }}</h3>
                    </p>
                    <p>
                        <span>{{ miEvento.evento.fecha }}</span>
                    </p>
                </div>
                <div>
                    <p>{{ miEvento.evento.texto }}</p>
                </div>

                <button :id="i" @click="archivar" class="btn btn-outline-light">Archivar</button>

            </li>
        </ol>
    </main>

</template>

<script>

    export default{
        created() {
            this.perfil();
        },
        data(){
            return{
                usuario:{
                    nombre: "",
                    apellido: "",
                    perfil: ""
                },
                evento:{
                    texto: "",
                    fecha: ""
                },
                lista: []
            }
        },
        methods: {
            perfil(){
                axios.get('https://randomuser.me/api/')
                .then(datos => datos.data.results[0])
                .then(datos => {
                    this.usuario.nombre = datos.name.first;
                    this.usuario.apellido = datos.name.last;
                    this.usuario.perfil = datos.picture.medium;
                });
            },
            crear(){
                (this.evento.texto && this.evento.fecha) && this.lista.push(
                    {
                        "usuario": Object.assign({}, this.usuario),
                        "evento": Object.assign({}, this.evento)
                    }
                );

                this.evento.texto = "";
                this.evento.fecha = "";
            },
            archivar(event){
                this.lista.splice(Number(event.target.id), 1);
            },
            generador(event){
                if (event.keyCode === 17) {
                    this.evento.texto = "It is a long established fact that a reader will be distracted by the readable content of a page when looking at its layout. The point of using Lorem Ipsum is that it has a more-or-less normal distribution of letters, as opposed to using 'Content here, content here', making it look like readable English";
                    this.evento.fecha = "2023-02-17T22:35";
                    this.crear();
                }
            }
        }
    }
</script>

<template>
    <div>
        <form @submit.prevent="agregar">
            <ion-item>
                <ion-label position="floating">Nombre</ion-label>
                <ion-input class="form-control" :value="nombre" @input= "nombre = $event.target.value"></ion-input>
            </ion-item>

            <ion-item>
                <ion-label position="floating">IP Local</ion-label>
                <ion-input class="form-control" :value="ipLocal" @input= "ipLocal = $event.target.value"></ion-input>
            </ion-item>

            <ion-item>
                <ion-label position="floating">IP Remota</ion-label>
                <ion-input class="form-control" :value="ipRemota" @input= "ipRemota = $event.target.value"></ion-input>
            </ion-item>

            <ion-card-content>
                <ion-button type="submit" expand="block">Agregar</ion-button>
            </ion-card-content>
            
            <ion-card>
                <ion-card-header>
                    <ion-card-subtitle>Conexiones</ion-card-subtitle>
                </ion-card-header>

                <ion-card-content>
                    <ion-grid>
                        <ion-row >
                            <ion-col size="3.5">
                            <ion-label style="color: black;">Nombre</ion-label> 
                            </ion-col>
                            <ion-col size="3.5">
                            <ion-label style="color: black;">Ip Local</ion-label> 
                            </ion-col>
                            <ion-col size="3.5">
                            <ion-label style="color: black;">Ip Remota</ion-label> 
                            </ion-col>
                        </ion-row>
                        <ion-row v-for="bomba of bombas" v-bind:key="bomba.ipLocal">
                            <ion-col size="3.5">
                                <ion-label>{{bomba.nombre}}</ion-label>
                            </ion-col>
                            <ion-col size="3.5">
                                <ion-label >{{bomba.ipLocal}}</ion-label>
                            </ion-col>
                            <ion-col size="3.5">
                                <ion-label >{{bomba.ipRemota}}</ion-label>
                            </ion-col>
                            <ion-col size="1.5">
                                <ion-icon style="font-size: 20px;" name="md-trash" color="danger" @click="eliminar(bomba.ipLocal)"></ion-icon>
                            </ion-col>
                        </ion-row>
                    </ion-grid>
                </ion-card-content>
            </ion-card>
        </form>
    </div>
</template>
<script lang="ts">
    export default {
        data(){
            return{
                bombas: [],
                nombre: '',
                ipLocal: '',
                ipRemota: '',

            }
        },
        created(){
            let datosDB= JSON.parse(localStorage.getItem('bombas'));
            if(datosDB === null){
                this.bombas = [];
            }else{
                this.bombas = datosDB;
            }
        },
        methods:{
            agregar(){
                this.bombas.push({
                    nombre: this.nombre,
                    ipLocal: this.ipLocal,
                    ipRemota: this.ipRemota,
                });
                this.nombre= '';
                this.ipLocal= '';
                this.ipRemota= '';
                localStorage.setItem('bombas', JSON.stringify(this.bombas));
            },
            eliminar(index){
                this.bombas.splice(index, 1);
                localStorage.setItem('bombas', JSON.stringify(this.bombas));
            }
        }
    }
</script>
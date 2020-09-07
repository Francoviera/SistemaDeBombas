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
                    <ion-card-subtitle>Molinos</ion-card-subtitle>
                </ion-card-header>

                <ion-card-content>
                    <ion-list>
                        <ion-item v-for="bomba of bombas" v-bind:key="bomba.ipLocal">
                            <ion-label>{{bomba.nombre}}</ion-label>
                            <ion-label size="small">{{bomba.ipLocal}}</ion-label>
                            <ion-button size="small" color="danger" @click="eliminar(bomba.ipLocal)">Eliminar</ion-button>
                        </ion-item>
                    </ion-list>
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
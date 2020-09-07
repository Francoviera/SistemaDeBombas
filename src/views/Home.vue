<template>
  <div class="home">
    <ion-card>
      <ion-card-header>
        <ion-card-title>Estado Actual</ion-card-title>
      </ion-card-header>

      <ion-card-content>
        <ion-item>
          <ion-label>Conexion</ion-label>
          <ion-select  placeholder="Seleccione" :value="conexion_local" @ionChange="conexion_local= $event.target.value">
            <ion-select-option value= "local">Local </ion-select-option>
            <ion-select-option value= "remota">Remota </ion-select-option>
          </ion-select>   
        </ion-item>
      </ion-card-content>
    </ion-card>

    <ion-card v-if="cargando">
      <ion-card-content class="contenedor">
        <ion-item class="preloader" >
        </ion-item>
      </ion-card-content>
    </ion-card>
     <ion-card v-if="!cargando">
      <ion-card-content>
        <ion-item v-for="(bomba, index) of estadosBombas" :key="index">
            <ion-label> {{bomba.bomba.nombre}}</ion-label>
            <ion-icon style="margin-right: 15px;" color="primary" name="water"></ion-icon>
            <ion-button v-if="bomba.data.bomba === false" color="success" @click="encenderBomba(bomba.bomba)"> encender</ion-button>
            <ion-button v-else color="danger" @click="apagarBomba(bomba.bomba)"> Apagar</ion-button>
        </ion-item>
      </ion-card-content>
      <!-- <Tanque :nombre='tanque.nombre' :data='tanque.data' />  -->
    </ion-card>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from 'axios';
// import Tanque from '@/components/Tanque.vue'

export default {
  // components: {
  //   Tanque
  // },
  data: function(){
      return{
        cargando: true,
        // verEstado: false,
        bombas: [],
        estadosBombas: [],
        conexion_local: "local",
        // tanque: ''
      }
    },
    mounted(){
      let datosDB= JSON.parse(localStorage.getItem('bombas'));
      if(datosDB === null){
        this.bombas = [];
      }else{
        this.bombas = datosDB;
      }
      this.getBombasEstados();
    },
    methods:{
      getBombasEstados(){
        if(this.conexion_local === "local"){
          for (const bomba of this.bombas) {
            this.getbomba('http://'+bomba.ipLocal, bomba);
          }
        }
        if(this.conexion_local === "remota"){
          for (const bomba of this.bombas) {
            this.getbomba('http://'+bomba.ipRemota, bomba);
          }
        }
      },
       async getbomba (url, bomba) { 
        await axios.get(url).then(response=>{
          if(this.estadosBombas != []){
            this.estadosBombas= []
          }
          this.estadosBombas.push({'bomba': bomba, 'data': response.data});
          this.cargando= false; 
        }).catch(() => {
          alert("No se puede conectar al Servidor, Verifique su conexion!")
        });
      },
      encenderBomba(bomba){
        console.log(bomba);
        if(this.conexion_local === 'local'){
          axios.get('http://'+bomba.ipLocal+'/'+bomba.nombre+'-ON').then(()=>{
            this.getBombasEstados();
          }).catch(() => {
            alert("No se pudo Encender!")
          });
        }else{
          axios.get('http://'+bomba.ipRemota+'/'+bomba.nombre+'-ON').then(()=>{
          this.getBombasEstados();
          }).catch(() => {
            alert("No se pudo Encender!")
          });
        }
      },
      apagarBomba(bomba){
        console.log(bomba);
        if(this.conexion_local === 'local'){
          axios.get('http://'+bomba.ipLocal+'/'+bomba.nombre+'-OFF').then(()=>{
            this.getBombasEstados();
          }).catch(() => {
            alert("No se pudo Apagar!")
          });
        }else{
          axios.get('http://'+bomba.ipRemota+'/'+bomba.nombre+'-OFF').then(()=>{
          this.getBombasEstados();
          }).catch(() => {
            alert("No se pudo Apagar!")
          });
        }
      },
      // vistaEstadoTanque(nombre, data){
      //   this.tanque.nombre= nombre;
      //   this.tanque.data= data;
      // }
    }
}
</script>
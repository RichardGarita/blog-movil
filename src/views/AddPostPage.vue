<template>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-back-button :text="getBackButtonText()" default-href="/"></ion-back-button>
        </ion-buttons>
        <ion-title>Nueva Publicación</ion-title>
      </ion-toolbar>
    </ion-header>
    <ion-grid>
      <ion-row class="ion-justify-content-center ion-align-items-center">
        <ion-col size="12" size-sm="8" size-md="6" size-lg="6">
          <form>
              <ion-item>
                  <ion-input v-model="newPost.title" label="Titulo" label-placement="stacked" placeholder="Escriba su titulo"></ion-input>
              </ion-item>
              <ion-item>
                  <ion-textarea v-model="newPost.body" label="Contenido" label-placement="stacked" placeholder="Escriba su contenido"></ion-textarea>
              </ion-item>
              <ion-button fill="solid" @click="createPost">Agregar</ion-button>
          </form>
      </ion-col>
      </ion-row>
    </ion-grid>
</template>

<script setup lang="ts">
import {
    IonInput,
    IonTextarea,
    IonItem,
    IonButton,
    IonButtons,
    IonBackButton,
    IonContent,
    IonHeader,
    IonList,
    IonPage,
    IonRefresher,
    IonRefresherContent,
    IonTitle,
    IonToolbar,
    IonFab,
    IonFabButton,
    IonIcon,
    IonGrid,
    IonCol,
    IonRow
} from '@ionic/vue';
import { ref } from 'vue';

defineProps({
    newPost: Object,
  });
  const newPost = ref({
  title: '',
  body: '',
});

const getBackButtonText = () => {
  const win = window as any;
  const mode = win && win.Ionic && win.Ionic.mode;
  return mode === 'ios' ? 'Inbox' : '';
};

const createPost = () => {
  if (!newPost.value.title.trim() || !newPost.value.body.trim()) {
    console.error('Los campos son obligatorios.');
    alert("Todos los campos son obligatorios");
    return;
  }
  fetch('http://localhost:3000/posts', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(newPost.value),
  })
    .then(response => response.json())
    .then(data => {
      console.log('Respuesta del servidor:', data);
      alert("Publicación creada con éxito!");
    })
    .catch(error => {
      console.error('Error al realizar la solicitud POST:', error);
      alert("Error al crear la publicación, trate de nuevo");
    });
}


</script>
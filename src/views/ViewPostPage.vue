<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-buttons slot="start">
          <ion-back-button :text="getBackButtonText()" default-href="/"></ion-back-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true" v-if="post">
      <ion-item>
        <ion-label class="ion-text-wrap">
          <h2>
            <h1>
              {{ post.title }}
            </h1>
            <span class="date">
              <ion-note>{{ post.createdAt }}</ion-note>
            </span>
          </h2>
        </ion-label>
      </ion-item>

      <div class="ion-padding">
        <p>
          {{post.body}}
        </p>
      </div>

      <ion-item>
        <ion-header>
          <h4>
            <strong>
              Comentarios
            </strong>
          </h4>
        </ion-header>
      </ion-item>

      <ion-card>
        <ion-grid>
          <ion-row>
            <ion-col size="4">
              <form>
                <ion-item>
                  <ion-input v-model="newComment.authorEmail" label="Correo" type="email" label-placement="stacked" placeholder="correo@ejemplo.com" required></ion-input>
                </ion-item>
                <ion-item>
                  <ion-input v-model="newComment.body" label="Comentario" type="text" label-placement="stacked" placeholder="Escribe tu comentario aquí" required></ion-input>
                </ion-item>
                <ion-button fill="solid" @click="createComment(post.id)">Agregar Comentario</ion-button>
              </form>
            </ion-col>
          </ion-row>
        </ion-grid>
      </ion-card>

      <ion-item v-for="comment in comments" :key="comment.id">
        <p>
          <strong>{{comment.authorEmail}}</strong>
          {{comment.body}}
        </p>
      </ion-item>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import { useRoute } from 'vue-router';
import {
  IonBackButton,
  IonButtons,
  IonContent,
  IonHeader,
  IonItem,
  IonLabel,
  IonNote,
  IonPage,
  IonToolbar,
  IonGrid,
  IonRow,
  IonCol,
  IonInput,
  IonCard,
  IonButton
} from '@ionic/vue';
import { ref } from 'vue';

defineProps({
    post: Object,
  });

  const newComment = ref({
  authorEmail: '',
  body: ''
})

const getBackButtonText = () => {
  const win = window as any;
  const mode = win && win.Ionic && win.Ionic.mode;
  return mode === 'ios' ? 'Inbox' : '';
};

const route = useRoute();
const post = ref([]);
const comments = ref([])
const loadPost = async (id) => {
  const response = await fetch(`http://localhost:3000/posts/${id}`);
  const data = await response.json();

  console.log(data);
  post.value = data;
};
const loadComments = async (id) => {
  const response = await fetch(`http://localhost:3000/comments/${id}`);
  const data = await response.json();

  console.log(data);
  comments.value = data;
};

const createComment = async (id) => {
  if (!newComment.value.authorEmail.trim() || !newComment.value.body.trim()) {
    console.error('Los campos son obligatorios.');
    alert("Todos los campos son obligatorios");
    return;
  }
  fetch(`http://localhost:3000/comments/${id}`, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
    },
    body: JSON.stringify(newComment.value),
  })
    .then(response => response.json())
    .then(data => {
      console.log('Respuesta del servidor:', data);
      alert("Comentario creado con éxito!");
      window.location.reload();
    })
    .catch(error => {
      console.error('Error al realizar la solicitud POST:', error);
      alert("Error al crear el comentario, trate de nuevo");
    });
}
loadPost(route.params.id);
loadComments(route.params.id);
</script>

<style scoped>
ion-item {
  --inner-padding-end: 0;
  --background: transparent;
}

ion-label {
  margin-top: 12px;
  margin-bottom: 12px;
}

ion-item h2 {
  font-weight: 600;
  
  /**
   * With larger font scales
   * the date/time should wrap to the next
   * line. However, there should be
   * space between the name and the date/time
   * if they can appear on the same line.
   */
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

ion-item .date {
  align-items: center;
  display: flex;
}

ion-item ion-icon {
  font-size: 42px;
  margin-right: 8px;
}

ion-item ion-note {
  font-size: 0.9375rem;
  margin-right: 12px;
  font-weight: normal;
}

h1 {
  margin: 0;
  font-weight: bold;
  font-size: 1.4rem;
}

p {
  line-height: 1.4;
}
</style>

<template>
  <ion-page>
    <ion-header :translucent="true">
      <ion-toolbar>
        <ion-title>Publicaciones</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content :fullscreen="true">
      <ion-fab slot="fixed" vertical="bottom" horizontal="end">
      <ion-fab-button  @click="goToAddPost">
        <ion-icon :icon="add" ></ion-icon>
      </ion-fab-button>
      </ion-fab>
      <ion-refresher slot="fixed" @ionRefresh="refresh($event)">
        <ion-refresher-content></ion-refresher-content>
      </ion-refresher>

      <ion-header collapse="condense">
        <ion-toolbar>
          <ion-title size="large">Publicaciones</ion-title>
        </ion-toolbar>
      </ion-header>
      

      <ion-list>
        <PostListItem v-for="post in posts" :key="post.id" :post="post" />
      </ion-list>
    </ion-content>
  </ion-page>
</template>

<script setup lang="ts">
import {
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
} from '@ionic/vue';
import { add } from 'ionicons/icons';
import router from '../router/index';
import PostListItem from '@/components/PostListItem.vue';
import { ref } from 'vue';

const posts = ref([]);

const loadPosts = async () => {
  const response = await fetch('http://localhost:3000/posts');
  const data = await response.json();

  console.log(data);
  posts.value = data;
}

loadPosts();

const goToAddPost = async () => {
  router.push({ name: 'AddPost' });
}

const refresh = (ev: CustomEvent) => {
  setTimeout(() => {
    loadPosts();
    ev.detail.complete();
  }, 3000);
};
</script>

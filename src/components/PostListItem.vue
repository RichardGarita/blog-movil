<template>
    <ion-card v-if="post" :routerLink="'/post/' + post.id" :detail="false" class="list-item">
        <ion-card-header>
            <ion-card-subtitle>
            {{ post.createdAt }}
            </ion-card-subtitle>
            <ion-card-title>
                {{ post.title }}
            </ion-card-title>
        </ion-card-header>
        <ion-card-content>
            {{post.body}}
            <ion-fab vertical="bottom" horizontal="end" slot="fixed">
              <ion-fab-button @click.prevent="deletePost(post.id)" color="danger">
                <ion-icon :icon="trash" aria-label="Borrar" ></ion-icon>
              </ion-fab-button>
            </ion-fab>
        </ion-card-content>
    </ion-card>
  </template>
  
  <script setup lang="ts">
  import { IonCard, IonCardHeader, IonCardTitle, IonCardSubtitle, IonCardContent, IonIcon, IonFab, IonFabButton } from '@ionic/vue';
  import { trash } from 'ionicons/icons';
  
  defineProps({
    post: Object,
  });
  
  const deletePost = async (postId) => {
    const response = await fetch(`http://localhost:3000/posts/${postId}`, {
      method: 'Delete',
      headers: {
        'Content-Type': 'application/json'
      }
    })
      .then(response => {
        console.log("Respuesta del servidor: ", response);
        alert('Se borró el la publicación');
        window.location.reload();
      })
      .catch(error => {
        console.error(error);
        alert("Hubo un error al borrar la publicación");
      })

  };
  </script>
  
  <style scoped>
  .list-item {
    --padding-start: 0;
    --inner-padding-end: 0;
  }
  
  .list-item ion-label {
    margin-top: 12px;
    margin-bottom: 12px;
  }
  
  .list-item h2 {
    font-weight: 600;
    margin: 0;
    
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
  
  .list-item p {
    text-overflow: ellipsis;
    overflow: hidden;
    white-space: nowrap;
    width: 95%;
  }
  
  .list-item .date {
    align-items: center;
    display: flex;
  }
  
  .list-item ion-icon {
    color: #c9c9ca;
  }
  
  .list-item ion-note {
    font-size: 0.9375rem;
    margin-right: 8px;
    font-weight: normal;
  }
  
  .list-item ion-note.md {
    margin-right: 14px;
  }
  
  .list-item .dot {
    display: block;
    height: 12px;
    width: 12px;
    border-radius: 50%;
    align-self: start;
    margin: 16px 10px 16px 16px;
  }
  
  .list-item .dot-unread {
    background: var(--ion-color-primary);
  }
  </style>
  
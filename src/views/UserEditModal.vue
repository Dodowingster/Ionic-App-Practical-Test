<template>
  <ion-modal :is-open="isVisible" @didDismiss="emitClose">
    <ion-header>
      <ion-toolbar>
        <ion-title>Edit User</ion-title>
        <ion-buttons slot="end">
          <ion-button @click="emitClose">Close</ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <ion-item>
        <ion-label>Name:</ion-label>
        <ion-input v-model="localUser.name" placeholder="Enter username"></ion-input>
      </ion-item>
      <ion-item>
        <ion-label>Email:</ion-label>
        <ion-input v-model="localUser.email" placeholder="Enter email"></ion-input>
      </ion-item>
      <ion-item>
        <ion-label>Mobile:</ion-label>
        <ion-input v-model="localUser.mobile" placeholder="Enter mobile number"></ion-input>
      </ion-item>
      <ion-button expand="full" @click="saveUser">Save</ion-button>
    </ion-content>
  </ion-modal>
</template>

<script>
import { defineComponent, ref, watch } from 'vue';
import { IonModal, IonHeader, IonToolbar, IonTitle, IonButtons, IonButton, IonContent, IonItem, IonLabel, IonInput } from '@ionic/vue';
import { environment } from '../environments/environment_dev';

export default defineComponent({
  components: { IonModal, IonHeader, IonToolbar, IonTitle, IonButtons, IonButton, IonContent, IonItem, IonLabel, IonInput },
  props: {
    isVisible: {
      type: Boolean,
      required: true
    },
    user: {
      type: Object,
      required: true
    }
  },
  setup(props, { emit }) {
    const localUser = ref({ ...props.user });

    watch(() => props.user, (newUser) => {
      localUser.value = { ...newUser };
    });

    const emitClose = () => {
      emit('update:isVisible', false);
    };

    const saveUser = async () => {
      try {
        const response = await fetch(`${environment.apiUrl}users/${localUser.value.id}/`, {
          method: 'PUT',
          headers: {
            'Content-Type': 'application/json',
          },
          body: JSON.stringify(localUser.value),
        });

        if (!response.ok) {
          throw new Error('Failed to update user');
        }

        const updatedUser = await response.json();
        emit('updateUser', updatedUser);
        emitClose();
      } catch (error) {
        console.error("Error updating user:", error);
      }
    };

    return { localUser, emitClose, saveUser };
  }
});
</script>

<template>
  <ion-modal :is-open="isVisible" @didDismiss="emitClose">
    <ion-header>
      <ion-toolbar>
        <ion-title>Add User</ion-title>
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
      <ion-button expand="full" @click="addUser">Add</ion-button>
    </ion-content>
  </ion-modal>
</template>

<script>
import { defineComponent, ref } from 'vue';
import { IonModal, IonHeader, IonToolbar, IonTitle, IonButtons, IonButton, IonContent, IonItem, IonLabel, IonInput } from '@ionic/vue';
import { environment } from '../environments/environment_dev';

export default defineComponent({
  components: { IonModal, IonHeader, IonToolbar, IonTitle, IonButtons, IonButton, IonContent, IonItem, IonLabel, IonInput },
  props: {
    isVisible: {
      type: Boolean,
      required: true
    }
  },
  setup(props, { emit }) {
    const localUser = ref({
      name: '',
      email: '',
      mobile: ''
    });

    const emitClose = () => {
      emit('update:isVisible', false);
    };

    const addUser = async () => {
      if (!localUser.value.name || !localUser.value.email || !localUser.value.mobile) {
        alert('All fields are required.');
        return;
      }

      try {
        const response = await fetch(`${environment.apiUrl}users/`, {
        method: 'POST',
        headers: {
            'Content-Type': 'application/json',
        },
        body: JSON.stringify(localUser.value),
        });

        if (!response.ok) {
        throw new Error('Failed to add user');
        }

        const newUser = await response.json();
        emit('userAdded', newUser);
        emitClose();
      } catch (error) {
        console.error("Error adding user:", error);
      }
    };

    return { localUser, emitClose, addUser };
  }
});
</script>

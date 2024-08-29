<template>
  <ion-page>
    <ion-header>
      <ion-toolbar>
        <ion-title>Contact List</ion-title>
        <ion-buttons slot="end">
          <ion-button @click="openUserAddModal">Add User</ion-button>
        </ion-buttons>
      </ion-toolbar>
    </ion-header>
    <ion-content>
      <ion-list>
        <ion-item v-for="user in users" :key="user.id" @click="showActionSheet(user)">
          <ion-label>
            <h2>{{ user.name }}</h2>
            <p>{{ user.email }}</p>
            <p>{{ user.mobile }}</p>
          </ion-label>
        </ion-item>
      </ion-list>

      <!-- Action Sheet -->
      <ion-action-sheet v-if="actionSheetVisible" :header="`Actions for ${selectedUser?.name}`" :buttons="actionSheetButtons" @didDismiss="actionSheetDismissed"></ion-action-sheet>
      
      <!-- User Edit Modal -->
      <UserEditModal
        :isVisible="editModalVisible"
        :user="selectedUser"
        @update:isVisible="editModalVisible = $event"
        @updateUser="handleUserUpdate"
      />

      <!-- Add User Modal -->
      <UserAddModal
        :isVisible="UserAddModalVisible"
        @update:isVisible="UserAddModalVisible = $event"
        @userAdded="handleUserAddition"
      />
    </ion-content>
  </ion-page>
</template>

<script>
import { IonActionSheet } from '@ionic/vue';
import { ref } from 'vue';
import { actionSheetController } from '@ionic/vue';
import UserEditModal from './UserEditModal.vue';
import UserAddModal from './UserAddModal.vue';
import { environment } from '../environments/environment_dev';


export default {
  components: { IonActionSheet, UserEditModal, UserAddModal },
  setup() {
    const users = ref([]);
    const actionSheetVisible = ref(false);
    const editModalVisible = ref(false);
    const UserAddModalVisible = ref(false);
    const selectedUser = ref(null);

    const actionSheetButtons = [
      {
        text: 'Delete',
        role: 'destructive',
        data: {
          action: 'delete',
        },
      },
      {
        text: 'Edit',
        data: {
          action: 'edit',
        },
      },
    ];

    const showActionSheet = async (user) => {
      selectedUser.value = user;
      const actionSheet = await actionSheetController.create({
        header: `Actions for ${user.name}`,
        buttons: actionSheetButtons,
      });
      await actionSheet.present();
      const { role } = await actionSheet.onDidDismiss();
      handleAction(role, user);
    };

    const handleAction = async (role, user) => {
      const action = actionSheetButtons.find(button => button.role === role)?.data?.action;
      switch (action) {
        case 'delete':
          await deleteUser(user.id);
          break;
        case 'edit':
          editModalVisible.value = true;
          break;
        default:
          break;
      }
    };

    const deleteUser = async (userId) => {
      try {
        await fetch(`${environment.apiUrl}users/${userId}/`, {
          method: 'DELETE',
        });
        users.value = users.value.filter(user => user.id !== userId);
      } catch (error) {
        console.error("Error deleting user:", error);
      }
    };

    const handleUserUpdate = (updatedUser) => {
      users.value = users.value.map(user => user.id === updatedUser.id ? updatedUser : user);
    };

    const handleUserAddition = (newUser) => {
      users.value.push(newUser);
    };

    const fetchUsers = async () => {
      try {
        const response = await fetch(`${environment.apiUrl}users/`);
        users.value = await response.json();
      } catch (error) {
        console.error("Error fetching users:", error);
      }
    };

    fetchUsers();

    const openUserAddModal = () => {
      UserAddModalVisible.value = true;
    };

    return { users, actionSheetButtons, showActionSheet, editModalVisible, UserAddModalVisible, selectedUser, handleUserUpdate, handleUserAddition, openUserAddModal };
  },
};
</script>

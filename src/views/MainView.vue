<script setup lang="ts">
import TableDays from '@/components/Cra/TableDays.vue';
import UserInputs from '@/components/User/UserInputs.vue';
import TheFooter from '@/components/HeaderFooter/TheFooter.vue';
import CostsAndComments from '@/components/CostsComments/CostsAndComments.vue';
import { useDaysStore } from '@/stores/daysStore';
import { useUserStore } from '@/stores/userStore';
import { createOrUpdateCraDB } from '@/services/cras'
import { ref } from '@vue/reactivity';
import router from '@/router';

const isOpened = ref(false);
const daysStore = useDaysStore();
const userStore = useUserStore();

const openModal = () => {
  if (userStore.user.isLogged) {
    isOpened.value = true;
  } else {
    router.push('/print');
  }
}

const validation = () => {
  createOrUpdateCraDB();
  isOpened.value = false;
  router.push('/print');
}

</script>

<template>
  <UserInputs />
  <TableDays v-if="!daysStore.signed"/>
  <!-- <CostsAndComments v-if="!daysStore.signed"/> -->

  <div class="modal-container" :class="{'opened': isOpened, 'closed': !isOpened }">
    <div class="modal">
      <div class="modal-header">Confirmation</div>
      <div class="modal-content">En validant, votre cra sera enregistré en tant que brouillon</div>
      <div class="modal-buttons">
        <button @click="validation">Valider</button>
        <button @click="isOpened = false">Annuler</button>
      </div>
    </div>
  </div>

  <div class="button">
    <button v-if="!daysStore.signed" @click="openModal" class="link" to="/print">
      Valider
    </button>
  </div>
  <div v-if="daysStore.signed">
    <h1 class="warn">Ce cra est déjà signé, vous ne pouvez plus le modifier</h1>
    <div class="button">
      <router-link class="link" to="/print">Voir le cra</router-link>
    </div>
  </div>
  <TheFooter />
</template>

<style scoped lang="scss">

.opened {
  visibility: visible;
}
.closed {
  visibility: hidden;
}
.modal-container {
  z-index: 1;
  background-color: rgba(0, 0, 0, 0.7);
  top: 0;
  position: absolute;
  min-height: 100%;
  min-width: 100%;
  & .modal {
    border: 1px solid black;
    border-radius: 5px;
    background-color: #f1f1f1;
    margin: 0 auto;
    margin-top: 10%;
    width: 300px;
    height: 135px;
    & .modal-header {
      height: 35px;
      border-bottom: 1px solid grey;
      padding-left: 5px;
      font-size: 25px;
      font-family: 'Bau-bold';
      color: black;
    }
    & .modal-content {
      padding-left: 5px;
      font-family: 'bau-regular';
      color: black;
    }
    & .modal-buttons {
      margin: 0 auto;
      text-align: center;
      margin-top: 10px;
      margin-bottom: 5px;
      & button {
        &:not(last-child) {
          margin-right: 5px;
        }
        height: 28px;
        vertical-align: top;
        border-radius: 3px;
        border: 1px solid transparent;
        background: #fff;
        border-color: #000;
        color: #000;
        cursor: pointer;
        &:hover {
          background: $main-color;
          color: white;
        }
      }
    }
  }
}
.warn {
  color: $main-color;
  margin: 0 auto;
  width: 50%;
}
.button {
  color: $main-color;
  margin: 0 auto;
  margin-top: 20px;
  width: 50%;
  text-align: center;
  & .link {
    background-color: #f1f1f1;
    padding: 5px 10px 5px 10px;
    border: 1px solid $main-color;
    border-radius: 5px;
    text-decoration: none;
    &:hover {
      background-color: $main-color;
      color: #f1f1f1;
    }
    &:visited {
      color:none;
    }
  }
}
</style>

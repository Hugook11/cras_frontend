<script setup lang="ts">
import { computed, ref } from '@vue/reactivity';
import { useUserStore } from '@/stores/userStore';
import { useDaysStore } from '@/stores/daysStore';
import { signCra } from '@/services/cras'

const store = useUserStore();
const daysStore = useDaysStore();
const isOpened = ref(false);

const signatureDate = computed(() => {
  return `${daysStore.getSignatureDate}`
})
const signatureURL = computed(() => {
  return store.user.signatureURL;
})

const isLoggedIn = computed(() => {
  return store.user.isLogged
})

const sign = async () => {
  isOpened.value = false;
  await signCra()
}

</script>

<template>
  <div class="modal-container" :class="{'opened': isOpened, 'closed': !isOpened }">
    <div class="modal">
      <div class="modal-header">Confirmation</div>
      <div class="modal-content">En signant, votre cra sera enregistré et vous ne pourrez plus le modifier</div>
      <div class="modal-buttons">
        <button @click="sign">Signer</button>
        <button @click="isOpened = false">Annuler</button>
      </div>
    </div>
  </div>

  <div class="signatures">
    <div>
      <p>Date et signature de l'intervenant</p>
      <button v-if="!daysStore.signed && isLoggedIn" @click="isOpened = !isOpened">Signer</button>
      <p v-if="daysStore.signed" class="date">{{ signatureDate }}</p>
      <img v-if="daysStore.signed" :src="signatureURL">
    </div>
    <div>Date et signature du responsable client</div>
  </div>
</template>

<style lang="scss">
.signatures {
  display: flex;
  flex-direction: row;
  margin: 0 auto;
  height: 140px;
  width: 33%;
  & .date{
    color: black;
  }
  & div {
    border: 1px solid $main-color;
    width: 50%;
    margin-top: 20px;
    padding: 8px;
    text-align: center;
    font-size: 10px;
    font-family: "Bau-Bold", Arial, Helvetica, sans-serif;
    color: #790053;
    & p {
      margin-top: 0px;
      margin-bottom: 0px;
    }
    & img {
      height: 85%;
    }
    &:nth-of-type(1) {
      margin-right: 5px;
    }
  }
}
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
    height: 160px;
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
</style>
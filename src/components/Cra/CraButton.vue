<script setup lang="ts">
import router from '@/router';
import { getCra } from '@/services/cras';

const props = defineProps<{ month: string, signed: boolean }>();
const date = new Date(props.month);
const year = date.getFullYear();
const localeDateString = `${date.toLocaleString('default', { month: 'long' })} ${year}`;

const getThisCra = async () => {
  await getCra(props.month);
  props.signed ? router.push('/print') : router.push('/');
}

</script>

<template>
<div class="cras-button-container">
  <div class="button" :id="month">
    <div class="month">{{ localeDateString }}</div>
    <div class="status" v-if="!signed">Brouillon <button @click="getThisCra()">Editer</button></div>
    <div class="status" v-if="signed">Signé <button @click="getThisCra()">Voir le cra</button></div>
  </div>
</div>
</template>

<style scoped lang="scss">
.cras-button-container {
  width: 100%;
  & .button {
    display: flex;
    flex-direction: row;
    margin: 0 auto;
    border: 1px solid black;
    border-radius: 5px;
    height: 40px;
    min-width: 330px;
    max-width: 500px;
    margin-bottom: 5px;
    padding-top: 5px;
    & div {
      margin: 0 auto;
      &.month{
        flex: 1;
      }
      &.status{
        flex: 1;
        & button {
          height: 34px;
          vertical-align: top;
          border-radius: 3px;
          border: 1px solid transparent;
          transition: all .3s ease;
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
}
</style>

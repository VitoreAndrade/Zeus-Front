<template>
    <footer class="fotter-final">
      <p>Criado por Vitor Andrade © 2024. Todos os direitos reservados. Gerado em: {{ tempoDeCarregamento }}ms</p>
    </footer>
  </template>
  <script lang="ts">
import { defineComponent } from 'vue';

  export default defineComponent ({
    name:"RodapeFinal",
    data() {
      return {
        tempoDeCarregamento: 0
      };
    },
    mounted() {
      const tempoInicial = performance.now();
      window.addEventListener('load', () => {
        const tempoFinal = performance.now();
        this.tempoDeCarregamento = Math.round(tempoFinal - tempoInicial);
      });
    }
  });
  </script>
  <style scoped>
    .fotter-final{
    background-color: black;
    height: 6rem;
    display: flex;
    align-items: center;
    justify-content: center;
    }
  </style>
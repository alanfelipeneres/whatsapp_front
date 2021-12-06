<template>
  <div class="empty">
    <div class="container full-width full-height column items-center justify-center">
      <q-img src="../../../src/assets/connected.jpg" height="250px" width="250px"></q-img>
      <h4>Mantenha o celular conectado</h4>
      <span>
        O Whatsapp conecta ao seu telefone para sincronizar suas mensagens.
        Para reduzir o uso de dados, conecte seu telefone a uma rede Wi-Fi.
      </span>
    </div>
  </div>
</template>

<script>
import { defineComponent } from "vue";

export default defineComponent({
  name: "EmptyChat",
});
</script>

<style scoped lang="scss">
.empty {
  height: calc(100);
  min-width: 800px;
  width: calc(100vw - 410px);
  background-color: $empty-background;
}

.container {
  h4 {
    width: 350px;
    text-align: center;
    margin: 12px 0px;
    color: $font-empty;
  }

  span {
    width: 450px;
    text-align: center;
    color: $font-gray;
  }
}
</style>

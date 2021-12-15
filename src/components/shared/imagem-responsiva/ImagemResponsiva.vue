<template>
    <img class="imagem-responsiva" :src="url" :alt="titulo">
</template>

<script>

    export default{
        //Aqui são as propriedades que esse elemento poderá receber
        //que serão informados por quem chamar esse componente
        props: ['url', 'titulo']
    }
</script>

<style scoped>
  .imagem-responsiva{
    width: 100%;
  }
</style>

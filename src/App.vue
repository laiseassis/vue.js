<template>
  <div class="corpo">
    <h1 class="centralizado">{{ titulo }}</h1>

    <!-- V-on = O vue vai executar um evento, qual? O input...-->
    <!--O $event é especial do vue, e sabe tudo aquilo que foi disparado
    O target é quem disparou o evento...
    e o value é pra pegar o valor de quem disparou-->
    <input type="search" class="filtro" v-on:input="filtro = $event.target.value" placeholder="filtre por parte do titulo">
    {{filtro}}

    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto of fotosComFiltro">
        
        <meu-painel :titulo="foto.titulo">
          <imagem-responsiva :url="foto.url" :titulo="foto.titulo"> </imagem-responsiva>
        </meu-painel>      

      </li>
    </ul>
  </div>
</template> 

<script>
  import Painel from './components/shared/painel/Painel.vue';
  import ImagemResponsiva from './components/shared/imagem-responsiva/ImagemResponsiva.vue';
  
  export default {

    //Aqui estou dizendo que o componente painel deverá ser utilizado aqui 
    //assim ele vai la na pasta procurar ele
    components:{
      'meu-painel': Painel,
      'imagem-responsiva': ImagemResponsiva
    },

    //esse é o serne de tudo que acontece aqui
    data(){
      return{
        titulo: "AluraPic",
        fotos: [],
        filtro: '' 
      }
    }, 

  //aqui terão funções qe serão executadas por coisas usadas na view
    computed:{
      fotosComFiltro(){
          if(this.filtro){
            let exp = new RegExp(this.filtro.trim(), 'i');
            return this.fotos.filter(foto => exp.test(foto.titulo));
          } else {
            return this.fotos;
          }
      }
    },

    // created é um hook da lifecycle hooks
    //aqui dentro, ao carregar a tela, ele ja puxará o que
    //existe na api (caso contrario teria que ser ao
    //comando de um botão, por exemplo)
    created(){
      //O alert foi criado para testar se hook estava funcionando com o vue-resource
      //alert('criei o componente');

      // o http funciona pois vem direto do vue-resource
      let promise = this.$http.get('http://localhost:3000/v1/fotos');
      promise
        .then(res => res.json())
        .then(fotos => this.fotos = fotos, err => console.log(err));
    }
  } 
</script>

<style> 
  .corpo{
    font-family: Helvetica, sans-serif;
    width: 96%;
    margin: 0 auto;
  }

  .centralizado{
    text-align: center;
  }

  .lista-fotos{
    list-style: none;
  }
    
  .lista-fotos .lista-fotos-item{
    display: inline-block;
  }

  .filtro{
    display: block;
    width: 100%;
  }
</style>

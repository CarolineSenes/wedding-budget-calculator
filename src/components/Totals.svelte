<script>
  import Title from "./Title.svelte";

  //on exporte pour pouvoir modifier la valeur de total de l'extérieur du component (depuis App)
  //la valeur par défaut sera0
  export let total = 0;
</script>

<section class="container d-flex">
  <Title title="Total des dépenses :" />
  <div class="align-self-center mx-3 h2">{total}€</div>
</section>

<style>
</style>

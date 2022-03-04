<script>
  import Title from "./Title.svelte";

  //on exporte pour pouvoir modifier la valeur de total de l'extérieur du component (depuis App)
  //la valeur par défaut sera0
  export let total = 0;
</script>

<section class="container d-flex">
  <Title title="Total des dépenses" />
  <div class="totals align-self-center mx-3 h2 py-1 px-3 rounded-pill">{total}€</div>
</section>

<style>
  .totals{
    background-color: var(--primary-color);
    color: var(--white);
  }
</style>

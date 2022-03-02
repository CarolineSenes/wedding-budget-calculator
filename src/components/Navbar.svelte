<script>
    export let showForm;
</script>

<style>
    /* your styles go here */
</style>

<nav class="navbar navbar-light bg-light mb-5">
    <div class="container-fluid">
      <h1>Budget Calculator</h1>
      <form class="d-flex">
        <button class="btn btn-outline-dark me-2" type="button" on:click={showForm}>
          <i class="bi bi-plus-square" />
		  Ajouter une dépense
        </button>
      </form>
    </div>
</nav>

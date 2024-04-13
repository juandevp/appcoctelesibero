<script>
// @ts-nocheck
 	import Modal from './lib/Modal.svelte';

  let coctels = [];
  let pagina = 0;
	let showModal = false;
  let coctelnombre = 'N/A';

  async function loadCoctels() {
    const response = await fetch("https://www.thecocktaildb.com/api/json/v1/1/filter.php?c=Cocktail");
    const data = await response.json();
    console.log(data);
    coctels = data.drinks;
  }  
  
  loadCoctels();

  function siguientePagina() {
    pagina++;
    loadCoctels();
  }

  function anteriorPagina() {
    pagina--;
    loadCoctels();
  }

  function cambiarEstado(nombreCoctel){
    showModal = true; 
    coctelnombre = nombreCoctel;
  }
</script>

<h1 class="title">Preparar Cócteles y Sus Ingredientes</h1>

<div class="container">
  <div class="grid">
    {#each coctels as coctel}
        <div class="coctel">
          <h3>{coctel.strDrink}</h3>
          <!-- svelte-ignore a11y-missing-attribute -->
          <img src={coctel.strDrinkThumb}  width="80%"/>
          <button class="botonEstilo" on:click={cambiarEstado(coctel.strDrink)}>Ver Preparación</button>
      </div>
    {/each}
  </div>
  <Modal bind:showModal bind:coctelnombre></Modal>
</div>
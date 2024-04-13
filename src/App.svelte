<script>
 	import Modal from './lib/Modal.svelte';
  let characters = [];
  let pagina = 'a';

	let showModal = false;
  let coctelnombre = 'N/A';

  async function loadCharacters() {
    const response = await fetch(
      "https://www.thecocktaildb.com/api/json/v1/1/filter.php?c=Cocktail"    );
    const data = await response.json();
    console.log(data);
    characters = data.drinks;
  }  
  
  loadCharacters();

  function siguientePagina() {
    pagina++;
    loadCharacters();
  }

  function anteriorPagina() {
    pagina--;
    loadCharacters();
  }
  function cambiarEstado(nombreCoctel){
    showModal = true; 
    coctelnombre = nombreCoctel;
  }
</script>

<h1 class="title">Preparación de cócteles</h1>

<div class="container">
  <div class="grid">
    {#each characters as character}
   <div class="character">
  <img src={character.strDrinkThumb}  width="20%" />
  <button class= "botonEstilo" on:click={cambiarEstado(character.strDrink)}>{character.strDrink} </button>
  <!--
   <h3>{character.species}</h3> -->
</div>
    {/each}
  </div>
<Modal bind:showModal bind:coctelnombre>
  
</Modal>
</div>
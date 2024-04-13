<script>
// @ts-nocheck
 	import Modal from './lib/Modal.svelte';
  
  // Almacena un arreglo con los datos de los cocteles
  let coctels = [];
  // Bandera que indica si muestra el modal
	let showModal = false;
  // Variable que usa para saber que coctel se selciona    
  let coctelnombre = 'N/A';
  
  //Esta funcion se encarga de obtener los cocteles por medio de un llamado GET 
  async function loadCoctels() {
    const response = await fetch("https://www.thecocktaildb.com/api/json/v1/1/filter.php?c=Cocktail");
    const data = await response.json();
    console.log(data);
    coctels = data.drinks;
  }  
  
  loadCoctels();
  // Cambia el estado y asigna el nombre del coctel    
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
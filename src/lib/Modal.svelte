<script>
	// Se exportan las variables que se pueden usar por otros componentes 
	export let showModal; // boolean
	export let coctelnombre;
	// HTMLDialogElement
	let dialog; 
    // Esta condicion valida si a cambiado la variable showModal
	// y abre el modal y carga los datos de un coctel en particular  
	// $: indica que cuando cambie la variable se actualice el DOM
	$: if (dialog && showModal) {
		fetchData(coctelnombre)
    	dialog.showModal();
	}
	
	let data;
	// Obtiene los recursos de un coctel en particular
	export async function fetchData(idCoctel) {
		try {			  
		const response = await fetch('https://www.thecocktaildb.com/api/json/v1/1/search.php?s='+ idCoctel);
		if (!response.ok) {
			throw new Error('Error al obtener los datos');
		}
		data = await response.json();
		//Al recibir una respuesta HTTP 200 se mapean los campos
		// y por medio de getElementById se cambia los valores del modal 	
		document.getElementById("Titulo").innerHTML =data.drinks[0].strDrink+ '-'+data.drinks[0].strCategory;
		document.getElementById("Ingrediente_1").innerHTML = data.drinks[0].strIngredient1;
		document.getElementById("Ingrediente_2").innerHTML = data.drinks[0].strIngredient2;
		document.getElementById("Ingrediente_3").innerHTML = data.drinks[0].strIngredient3;
		document.getElementById("Ingrediente_4").innerHTML = data.drinks[0].strIngredient4;
		document.getElementById("Ingrediente_5").innerHTML = data.drinks[0].strIngredient5;
		document.getElementById("Instrucciones").innerHTML = data.drinks[0].strInstructions;
		} catch (error) {
			console.error(error);
		}
	}
    // Se encarga de inicializar variables al cerrar modal 
	function cerrarLimpiarModal(){
    	dialog.close();
		showModal = false
 	  	document.getElementById("Titulo").innerHTML = '' ;
		document.getElementById("Instrucciones").innerHTML = '';
		document.getElementById("Ingrediente_1").innerHTML = '';
		document.getElementById("Ingrediente_2").innerHTML = '';
		document.getElementById("Ingrediente_3").innerHTML = '';
		document.getElementById("Ingrediente_4").innerHTML = '';
		document.getElementById("Ingrediente_5").innerHTML = '';	
	}
</script>

<!-- svelte-ignore a11y-click-events-have-key-events a11y-no-noninteractive-element-interactions -->
<dialog bind:this={dialog} on:close={ cerrarLimpiarModal} on:click|self={cerrarLimpiarModal}>
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<div on:click|stopPropagation>
		<h3 id="Titulo">{coctelnombre}</h3>
		<h4>Ingredientes</h4>
		<ol class="definition-list">
			<li id="Ingrediente_1"></li>
			<li id="Ingrediente_2"></li>
			<li id="Ingrediente_3"></li>
			<li id="Ingrediente_4"></li>
			<li id="Ingrediente_5"></li>
		</ol>
		<h4>Instrucciones</h4>
		<p id="Instrucciones"></p>
		<!-- svelte-ignore a11y-autofocus -->
		<button class="botonEstilo" autofocus on:click={() => dialog.close()}>Cerrar</button>
	</div>
</dialog>

<style>
	dialog {
		max-width: 32em;
		border-radius: 0.5em;
		border: none;
		padding: 0;
		background-color: rgb(199, 202, 202);
	}
	dialog::backdrop {
		background: rgba(0, 0, 0, 0.3);
	}
	dialog > div {
		padding: 1em;
	}
	dialog[open] {
		animation: zoom 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
	}
	@keyframes zoom {
		from {
			transform: scale(0.95);
		}
		to {
			transform: scale(1);
		}
	}
	dialog[open]::backdrop {
		animation: fade 0.2s ease-out;
	}
	@keyframes fade {
		from {
			opacity: 0;
		}
		to {
			opacity: 1;
		}
	}
	button {
		display: block;
	}
</style>

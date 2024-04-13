<script>
	export let showModal; // boolean
	export let coctelnombre;
	let dialog; // HTMLDialogElement

	$: if (dialog && showModal) {
		fetchData(coctelnombre)
    	dialog.showModal();
	}
	
	let data;
	export async function fetchData(idCoctel) {
		try {			  
		const response = await fetch('https://www.thecocktaildb.com/api/json/v1/1/search.php?s='+ idCoctel);
		if (!response.ok) {
			throw new Error('Error al obtener los datos');
		}
		data = await response.json();
			console.log(data.drinks[0].strInstructions)
			console.log(document.getElementById("Instrucciones").innerHTML)
				document.getElementById("Titulo").innerHTML =data.drinks[0].strDrink+ '-'+data.drinks[0].strCategory;
				document.getElementById("Instrucciones").innerHTML = data.drinks[0].strInstructions;
		} catch (error) {
			console.error(error);
		}
	}

	function cerrarLimpiarModal(){
    	dialog.close();
		showModal = false
 	  	document.getElementById("Titulo").innerHTML = '' ;
		document.getElementById("Instrucciones").innerHTML ='';
	}
</script>

<!-- svelte-ignore a11y-click-events-have-key-events a11y-no-noninteractive-element-interactions -->
<dialog bind:this={dialog} on:close={ cerrarLimpiarModal} on:click|self={cerrarLimpiarModal}>
	<!-- svelte-ignore a11y-no-static-element-interactions -->
	<div on:click|stopPropagation>
		<h2 id = "Titulo">{coctelnombre}</h2>
		<ol class="definition-list">
			<li id = "Instrucciones"></li>
		</ol>
		<!-- svelte-ignore a11y-autofocus -->
		<button class= "botonEstilo" autofocus on:click={() => dialog.close()}>Cerrar</button>
	</div>
</dialog>

<style>
	dialog {
		max-width: 32em;
		border-radius: 0.2em;
		border: none;
		padding: 0;
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

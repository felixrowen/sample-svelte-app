<script>
	import Product from "./Product.svelte"
	import Button from "./Button.svelte"
	import Cart from "./Cart.svelte"

	let title = '';
	let price = 0;
	let description = '';
	let products = []
	let cartItems = []

	function setTitle(event) {
		title = event.target.value
	}

	function saveProduct() {
		const newProduct = {
			title: title,
			price: price,
			description: description
		}
		products = products.concat(newProduct)
	}

	function addToCart(event){
		const selectedTitle = event.detail;
		cartItems = cartItems.concat(
			{...products.find(prod => prod.title === selectedTitle)}
		);
	}
</script>

<style>
	section {
		width: 30rem;
		margin: auto;
	}

	label, 
	input,
	textarea {
		width: 100%;
	}
</style>

<section>
	<Cart items={cartItems} />
</section>

<hr />

<section>
	<div>
		<label for="title">Title</label>
		<input type="text" id="title" value="{title}" on:input={setTitle} />
	</div>
	<div>
		<label for="price">Price</label>
		<input type="number" id="price" bind:value={price} />
	</div>
	<div>
		<label for="description">Description</label>
		<textarea rows="3" id="description" bind:value={description}></textarea>
	</div>
	<Button on:click={saveProduct}>
		Create Product
	</Button>

	{#if products.length === 0}
		<p>No products were added yet!</p>
		{:else}
		{#each products as product}
			<Product 
				productTitle={product.title} 
				productPrice={product.price} 
				productDescription={product.description}
				on:addcart={addToCart}
			/>
		{/each}
	{/if}
</section>


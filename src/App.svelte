<script>
	import Eye from './Eye.svelte';

	let closed = false;
</script>

<style>
	.container {
		max-width: 700px;
		margin: auto;
	}
	h1 {
		text-align: center;
	}
	.eyes {
		display: flex;
		justify-content: space-around;
		width: 300px;
		margin: auto;
	}
	.form {
		margin-top: 50px;
		display: flex;
		flex-direction: column;
		align-items: center;
	}
	.control {
		margin-top: 10px;
	}
</style>

<div class="container">
	<h1>Login</h1>
	<div class="eyes">
		<Eye left="100px" closed={closed}  />
		<Eye left="50px" closed={closed} />
	</div>
	<div class="form">
		<div class="control">
			<label for="username">Username</label>
			<input name="username" type="text" />
		</div>
		<div class="control">
			<label for="password">Password</label>
			<input name="username" on:focus={() => closed = true} on:blur={() => closed = false} type="password" />
		</div>
	</div>
</div>

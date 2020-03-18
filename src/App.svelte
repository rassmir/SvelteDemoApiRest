<script>
	import {onMount} from 'svelte';
	import { fly } from 'svelte/transition';
	import MovieItem from './Movie/Item.svelte';
	const APIKEY = '29ed1d64cc3508c30f08131eb1860d99';
	const BASEURL = `https://api.themoviedb.org/3`;
	const APISETTINGS = `?api_key=${APIKEY}&language=es-MX`;

	let movies = [];
	function fetchMovies() {
		const URL = `${BASEURL}/discover/movie${APISETTINGS}&sort_by=popularity.desc`;
		fetch(URL)
		 .then(res => res.json())
		 .then(({results}) => {
		 	movies = results;
			 console.log(movies);
		})
	}
	onMount(async () => {
		console.log('the component has mounted');
	    fetchMovies()
	});

	let likedMovies= [];

	function togglelike(event) {
		console.log(event);
		const movie = event.detail;
		let index = likedMovies.findIndex(m => m.id === movie.id);
		if( index >= 0){
			likedMovies.splice(index,1);
		}else{
			likedMovies.push(movie);
			console.log(likedMovies)
		}
		likedMovies = likedMovies
	}
	$: like = (id) => {
		let index = likedMovies.findIndex(m =>m.id ===id);
		return index >=0;
	}
</script>
<svelte:head>
	<title>Movies Svelte</title>
	<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css" integrity="sha384-Vkoo8x4CGsO3+Hhxv8T/Q5PaXtkKtu6ug5TOeNV6gBiFeWPGFN9MuhOf23Q9Ifjh" crossorigin="anonymous">
</svelte:head>

<main class="container">
	<div class="row">
		<div class="col-12 col-md-6 col-lg-6 border panel" >
			<h2>Peliculas populares</h2>
		    <div class="row">
			  {#each movies as movie}
			  <div class="col-12 col-md-6 col-lg-4 p-2">
				<MovieItem like="{like(movie.id)}" id={movie.id} title={movie.title} overview={movie.overview} poster_path={movie.poster_path} on:onToggleLike={togglelike}/>
			  </div>
			   {/each}
			</div>
		</div>
		<div class="col-12 col-md-6 col-lg-6 border panel">
			<h2>Peliculas favoritas</h2>
            <div class="row">
				{#if likedMovies.length}
				{#each likedMovies as movie, i (movie.id)}
					<div in:fly="{{duration:2000,y:20}}" out:fly={{duration:800,y:-20}} class="col-12 col-md-6 col-lg-4 p-2">
						<MovieItem like="{like(movie.id)}" id={movie.id} title={movie.title} overview="" poster_path={movie.poster_path} on:onToggleLike={togglelike}/>
					</div>
				{/each}
				{:else}
                 <div class="col-12">
					 <p>No tienes peliculas favoritas.</p>
				 </div>
				{/if}
			</div>
		</div>
	</div>
</main>

<style>
	.panel{
		height:90vh;
		overflow: auto;
	}
	main {
		text-align: center;
		padding: 1em;
		max-width: 240px;
		margin: 0 auto;
	}

	h1 {
		color: red;
		text-transform: uppercase;
		font-size: 4em;
		font-weight: 100;
	}

	@media (min-width: 640px) {
		main {
			max-width: none;
		}
	}
</style>
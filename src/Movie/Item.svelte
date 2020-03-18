<script>
    import {createEventDispatcher} from "svelte";

    export let id;
    export let title;
    export let overview;
    export let poster_path;
    export let coverUrl;
    export let like;
    $: coverUrl = `https://image.tmdb.org/t/p/w500/${poster_path}`

    const dispatch=createEventDispatcher();

    function togglelike() {
        dispatch('onToggleLike',{id,title,overview,poster_path})
    }
</script>

<div class="card">
    <img src='{coverUrl}' class="card-img-top" alt="...">
    <div class="card-body">
        <h5 class="card-title">{title}</h5>
        <p class="card-text">{overview}</p>
    <div class="text-center">
     <button class="{like ? 'btn btn-success' : 'btn btn-primary'}" on:click={togglelike}>{like ? 'Ya no me gusta' : 'Me gusta'}</button>
    </div>
    </div>
</div>
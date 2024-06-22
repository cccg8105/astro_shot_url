<script>
    import ClipBoard from "./ClipBoard.svelte";

    let url = "";
    let urlResult = "";

    let show = false;
    
    const copy = () => {
        const app = new ClipBoard({target:document.getElementById("clipboard"),
        props:{urlResult}
    });
        app.$destroy();
    }
    const fetchUrl = async () => {
        const options = {
            method: 'post',
            headers: {
                accept: 'application/json',
                'content-type': 'application/json',
                Authorization: 'sk_uzTdiCa7fieyY0QK'
            },
            body: JSON.stringify({originalURL: url, domain: 'fq6n.short.gy'})
        };
        
        const response = await fetch('https://api.short.io/links', options);
        const data = await response.json();
        console.log(data);
        urlResult = data.shortURL;
        show = true
    }
    const styleInput = "bg-gray-200 appearance-none border-2 border-gray-200 rounded w-full py-3 px-4 text-gray-700 leading-tight focus:outline-none focus:bg-white focus:border-purple-500";

    const buttonstyle = "bg-purple-500 hover:bg-purple-400 text-white font-bold py-2 px-10 rounded"
</script>
<div class="md:flex md:items-center md-6">
    <div class="md:w-1/3">
        <label for="" class="block text-gray-500 font-bold md:text-right pr-4">
            URL to shorten
        </label>
    </div>
    <div class="md:w-1/3">
        <input type="text" class={styleInput} placeholder="Example://www.mysite.com" bind:value={url}/></div>
</div>
<div class="md:flex md:items-center mt-10 justify-center">
    <button type="button" class={buttonstyle} on:click={fetchUrl}>Short</button>
</div>
{#if show }
<div class="md:flex md:items-center mt-10 justify-center">
    <input bind:value={urlResult} class="text-3xl text-pink-500">
    <button class ={buttonstyle} on:click={copy}>Copy</button>
    <div id="clipboard"></div>
</div>
{/if}



<script context="module">

export const fetchStock = (async () => {
	const response = await fetch('https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=IBM&apikey=854ZIE26CTNZ5UPX')
    return await response.json()
	// console.log(response)
	})()

</script>
<!-- <div>
{#each Object.entries(fetchStock) as stock }
	<p>{stock}</p>
{/each}

</div> -->


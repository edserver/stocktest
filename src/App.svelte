<script lang="ts">
	// import onMount from 'svelte';
	import {onMount} from "svelte/internal";
	// export let logo:string;
	import Navigation from '../src/components/navigation.svelte';
	import {fetchData} from '../src/components/register.svelte';


// 	let ticket  = 'amc';
	let ticket = prompt("Search a ticket");
	let url = `https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=${ticket}&apikey=854ZIE26CTNZ5UPX`;
	// let url = `https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=tsla&apikey=854ZIE26CTNZ5UPX`;
	let holder = [];
	let holderInfo;
	let error = null;
	onMount(() =>{
		fetchData(url)
		.then((data) =>{
			holder = Object.entries(data['Time Series (Daily)']);
			holderInfo = data['Meta Data']['2. Symbol'];

		})
		.catch((e) => error = e)
	});


	let edz = function getPorcentageChange(oldNumber ,newNumber){
		let decreaseValue = oldNumber - newNumber;
		let total = ((decreaseValue / oldNumber) * 100);
		if (total > 0) {
			return "+" + Math.round(total);
		}else {
			return Math.round(total);
		}
	}
	
	let addCommn = function addcomoon(num){
		return num.toString().replace(/\B(?=(\d{3})+(?!\d))/g, ',')
	}

</script>


<div class="container">
	{#if error !== null}
	loading...
	{:else}
	
{#each holder as  [key, value], i}

	
	<div class="box">
		<div class="header">
			{holderInfo.toLocaleUpperCase()}
			<br/>
			{key}
		</div>
		
		<div class="today">
	<div>Open: ${holder[i][1]['1. open']}</div>
	<div>High: ${holder[i][1]['2. high']}</div>
	<div>Low: ${holder[i][1]['3. low']}</div>
	<div>Close: ${holder[i][1]['4. close']}</div>
	<div>Volume: {addCommn(holder[i][1]['5. volume'])}</div>
			</div>
		
			
		<div class="result">
		<div>{edz( holder[i][1]['1. open'],holder[i+1][1]['1. open'])}%</div>
		<div>{edz( holder[i][1]['2. high'],holder[i+1][1]['2. high'])}%</div>
		<div>{edz( holder[i][1]['3. low'],holder[i+1][1]['3. low'])}%</div>
		<div>{edz( holder[i][1]['4. close'],holder[i+1][1]['4. close'])}%</div>
		<div>{edz( holder[i][1]['5. volume'],holder[i+1][1]['5. volume'])}%</div>
		
</div>
</div>
{/each}
{/if}
</div>
	
<style>


.container {
	/* background:#04090a; */
	color: white;
	display: flex;
	flex-wrap: wrap;
	justify-content: space-around;
}

.box {
	background: #201c1c;
	/* margin: 16px; */
	/* flex: 2 1 auto; */
	/* padding-bottom: 20px; */
	margin-bottom: 20px;
	width: 22%;
	text-align: center;
	font-size: 26px;
	font-family: monospace, BlinkMacSystemFont, "Segoe UI", Roboto, Oxygen-Sans, Ubuntu, Cantarell, "Helvetica Neue", sans-serif;
}

.header,.today,.result {
	padding: 6px;
	margin: 6px;
}

.header {


}



.today {

}

.result {
	background: #eeff6d;
	color: black;
}



</style>



<script lang="ts">
	// import onMount from 'svelte';
	import {onMount} from "svelte/internal";
	// export let logo:string;
	import Navigation from '../src/components/navigation.svelte';
	import {fetchData} from '../src/components/register.svelte';
	import {fetchStockData} from '../src/components/fetchStockData.svelte';


	// let ticket  = 'tsla';
	let ticket = prompt("Search a ticket: ");
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

let url1 = `https://www.alphavantage.co/query?function=EMA&symbol=${ticket}&interval=daily&time_period=10&series_type=open&apikey=demo854ZIE26CTNZ5UPX`;
	// let url = `https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol=tsla&apikey=854ZIE26CTNZ5UPX`;
	let holder1 = [];
	let holderInfo1;
        error = null;
	onMount(() =>{
		fetchStockData(url1)
		.then((data1) =>{
			holder1 = Object.entries(data1["Technical Analysis: EMA"]);
			// holderInfo1 = data['Meta Data']['2. Symbol'];
			console.log(holder1[0][1])
			// console.log(holder1[0])

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
	<img  id="loading" src="https://c.tenor.com/FIzWAbQcjpYAAAAC/loading-splash.gif" alt="loading..."/>
	{:else}


	{#each holder as  [key, value], i}
	<div class="header centerContent">
		{holderInfo.toLocaleUpperCase()}
		<br/>
		{key}
	</div>
	
	<div class="today alignCenter borders">
		<h3>Today Price</h3>
		<div>Open: ${holder[i][1]['1. open']}</div>
		<div>High: ${holder[i][1]['2. high']}</div>
		<div>Low: ${holder[i][1]['3. low']}</div>
		<div>Close: ${holder[i][1]['4. close']}</div>
		<div>Volume: {addCommn(holder[i][1]['5. volume'])}</div>
	</div>
	
	<div class="result alignCenter borders">
		<h3>Change</h3>
		<span>{edz( holder[i][1]['1. open'],holder[i+1][1]['1. open'])}%</span>
		<span>{edz( holder[i][1]['2. high'],holder[i+1][1]['2. high'])}%</span>
		<span>{edz( holder[i][1]['3. low'],holder[i+1][1]['3. low'])}%</span>
		<span>{edz( holder[i][1]['4. close'],holder[i+1][1]['4. close'])}%</span>
		<span>{edz( holder[i][1]['5. volume'],holder[i+1][1]['5. volume'])}%</span>
	</div>

	<!-- <div class="ema alignCenter borders">
		<h3>Technical Analysis: EMA</h3>
		<span>EMA(10): $140.0843</span>
		<span>EMA(50): $1440.5324</span>
		<span>EMA(100): $158.8095</span>
		<span>EMA(150): $230.8532</span>
		<span>EMA(200): $230.8414</span>
	</div>

	<div class="sma alignCenter borders">
		<h3>Technical Analysis: SMA</h3>
		<span>SMA(10): $138.854</span>
		<span>SMA(50): $130.0293</span>
		<span>SMA(100): $228.593</span>
		<span>SMA(150): $430.844</span>
		<span>SMA(200): $650.321</span>
	</div>

	<div class="short alignCenter borders">
		<h3>Short Interest (Daily)</h3>
		<span>Total Share: 450,4907,46924</span>
		<span>Total Tradable Shares: 280,000000</span>
		<span>Short % Interest: +28%</span>
	</div> -->






{/each}
{/if}
</div>
	
<style>

	


#loading {
	position: absolute;
	left: 35%;
	
}

.centerContent {
	display: flex;
    flex-direction: column;
    align-items: center;
}

.alignCenter {
        display: flex;
        justify-content: center;
        flex-direction: column;
        align-items: center;
    }

    .borders {
        border: 1px solid black;
    }


.container {
    display: grid;
}

.header {
    border: 1px dotted red;
    display: grid;
    grid-column: 1/6;
    text-align: center;
}






</style>

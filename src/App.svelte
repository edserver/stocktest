<script lang="ts">
	// import onMount from 'svelte';
	import {onMount} from "svelte/internal";
	// export let logo:string;
	import Navigation from '../src/components/navigation.svelte';
	import {fetchData} from '../src/components/register.svelte';



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
	<img  id="loading" src="https://c.tenor.com/FIzWAbQcjpYAAAAC/loading-splash.gif"/>
	{:else}


{#each holder as  [key, value], i}
	
	<div class="box">
		<div class="header">
			{holderInfo.toLocaleUpperCase()}
			<br/>
			{key}
		</div>

		
		
		
		
		
		
		<div class="today">
	<div>Open: ${holder[i][1]['1. open']} 
	<span class="result">{edz( holder[i][1]['1. open'],holder[i+1][1]['1. open'])}%</span>
	</div>

	<div>High: ${holder[i][1]['2. high']} 
	<span class="result">{edz( holder[i][1]['2. high'],holder[i+1][1]['2. high'])}%</span>
	</div>

	<div>Low: ${holder[i][1]['3. low']} 
	<span class="result">{edz( holder[i][1]['3. low'],holder[i+1][1]['3. low'])}%</span>
	</div>

	<div>Close: ${holder[i][1]['4. close']} 
	<span class="result">{edz( holder[i][1]['4. close'],holder[i+1][1]['4. close'])}%</span>
	</div>

	<div>Volume: {addCommn(holder[i][1]['5. volume'])} 
	<span class="result">{edz( holder[i][1]['5. volume'],holder[i+1][1]['5. volume'])}%</span>
	</div>
	
			</div>
		
			

</div>
{/each}
{/if}
</div>
	
<style>

#loading {
	position: absolute;
	left: 35%;
	
}
.container {
	/* background: red; */
	display: grid;
	grid-template-columns: repeat(5, 1fr);
}


.header {
	background: white;
}
.today {
	/* background: blue; */
}

.result {
	padding: 10px;
}

.box {
	background: #929391;
	margin: 10px;
	font-size: 20px;
}





</style>



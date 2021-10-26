<script lang="ts">
	// import onMount from 'svelte';
	import {onMount} from "svelte/internal";
	// export let logo:string;
	import Navigation from '../src/components/navigation.svelte';
	import {fetchData} from '../src/components/register.svelte';


	// let ticket  = 'amc';
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
			// let test11 = holder[1][1]['1. open']
			// let test11 = holder;
			// let test22 = holder[2][1]['1. open']
			// console.log(`Today high:${test11} Yesterday high:${test22}`)
			// console.log(`Today Change: ${test11 - test22}`);
			// if(test11 > test22){
			// 	console.log('YES');
			// }else {
			// 	console.log('NO');
			// }

			// console.log(test11)
			// console.log(test11.length)


			// TO GET TODAY OPEN HIGH CLOSE VOLUME OF TODAY ON AN ARRAY
			// THEN MAKE ANOTHER ARRAY HOLDING THE SAME DATA BUT FROM THE PREVIEWS DAY


			// for(let i = 0; i < holder.length; i++){
			// 	let getTodayStat = test11[i][1]['1. open'];
			// 	let getyesterStat = test11[i+1][1]['1. open'];
			// 	// console.log(edz(getyesterStat,getTodayStat))
			// 	// console.log(`Today Stat: ${getTodayStat} Yesterday stat: ${getyesterStat}`)
			// }
			

			// console.log(
			// holder[1][1]['1. open'],
			// holder[1][1]['2. high'],
			//  holder[1][1]['3. low'],
			//   holder[1][1]['4. close'],
			//    holder[1][1]['5. volume']
			//    )

			//    console.log('----')

			//    console.log(
			// holder[2][1]['1. open'],
			// holder[2][1]['2. high'],
			//  holder[2][1]['3. low'],
			//   holder[2][1]['4. close'],
			//    holder[2][1]['5. volume']
			//    )
		})
		.catch((e) => error = e)
	});


	// function getInputValue (){
	// 	let inputVal = document.getElementById("searchStock").value;
    //         ticket = inputVal;
	// 		// console.log(`New Name: ${ticket}`);
	// 		if(!inputVal.trim()){
	// 			console.log('Please type a symbol...')
	// 		} else {
	// 			console.log("you searching for Symbol: " + ticket)
	// 		}
	// }
	
	let edz = function getPorcentageChange(oldNumber ,newNumber){
		// oldNumber The inital value
		// newNumber The value the change
		let decreaseValue = oldNumber - newNumber;
		let total = ((decreaseValue / oldNumber) * 100);
		return (Math.round(total));
	}

	// console.log(getPorcentageChange(100, 758))
	// getPorcentageChange(786.6600, 805.0000);
	// Math.random(-2.278260869565221);

</script>

<section>


<!-- 	
	<h1>Check Stock Daily Stats</h1>
	<input type="text" placeholder="Search symbol" id="searchStock">
	<button>Search</button> -->
	
	<!-- <input type="text" placeholder="Search symbol" on:change={clickMe}> -->
	
	{#if error !== null}
	loading...
{:else}
	
{#each holder as  [key, value], i}
<div class="container">
	
	<div class="box1">
		<div class="header">
			{holderInfo.toLocaleUpperCase()}
			<br/>
			{key}
		</div>
		
		<div class="today">
	<div class="box">Open: ${holder[i][1]['1. open']}</div>
	<div class="box">High: ${holder[i][1]['2. high']}</div>
	<div class="box">Low: ${holder[i][1]['3. low']}</div>
	<div class="box">Close: ${holder[i][1]['4. close']}</div>
	<div class="box">Volume: ${holder[i][1]['5. volume']}</div>
		</div>
		
		<div class="result">

		<div>{edz( holder[i][1]['1. open'],holder[i+1][1]['1. open'])}%</div>
		<div>{edz( holder[i][1]['2. high'],holder[i+1][1]['2. high'])}%</div>
		<div>{edz( holder[i][1]['3. low'],holder[i+1][1]['3. low'])}%</div>
		<div>{edz( holder[i][1]['4. close'],holder[i+1][1]['4. close'])}%</div>
		<div>{edz( holder[i][1]['5. volume'],holder[i+1][1]['5. volume'])}%</div>
		
</div>


	
	
		
		
	</div>

</div>

	

		{/each}
{/if}

</section>





<style>

	
.container {


	display: flex;
	flex-wrap: wrap;

}
.header,.today,.result {
	padding: 6px;
}

.header {
	box-shadow:inset 0px 0px 15px 3px #23395e;
	background:linear-gradient(to bottom, #2e466e 5%, #415989 100%);
	background-color:#2e466e;
	border-radius:17px;
	border:1px solid #cccccc;
	display:inline-block;
	color:#ffffff;
	font-family:Arial;
	font-size:15px;
	padding:6px 13px;
	text-decoration:none;
	text-shadow:0px 1px 0px #263666;
	text-align: center;
	margin-left: 100px;
}



.box1 {
	width: 30%;
	/* box-shadow: 0 10px 20px rgba(0,0,0,0.19), 0 6px 6px rgba(0,0,0,0.23); */
	/* background: #c1ff49; */
	background: white;
	margin: 10px;
}


.today {
	box-shadow: 0px 1px 0px 0px #fff6af;
	background:linear-gradient(to bottom, #ffec64 5%, #ffab23 100%);
	background-color:#ffec64;
	/* border-radius:6px; */
	/* border:1px solid #ffaa22; */
	color:#333333;
	font-family:Arial;
	font-size:20px;
	font-weight:bold;
	padding:6px 24px;
	text-decoration:none;
	text-shadow:0px 1px 0px #ffee66;
	text-align: center;
}

.result {
	box-shadow: 0px 10px 14px -7px #276873;
	background:linear-gradient(to bottom, #599bb3 5%, #408c99 100%);
	background-color:#599bb3;
	/* border-radius:8px; */
	color:#ffffff;
	font-family:Arial;
	font-size:20px;
	font-weight:bold;
	padding:13px 32px;
	text-decoration:none;
	text-shadow:0px 1px 0px #3d768a;
	text-align: center;
}



</style>



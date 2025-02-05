<script>
	let displayed = $state("");
	let equalstate = $state(0);
	let buttons = ["CE","C", "(", ")", "7","8","9","/","4","5","6","x","1","2","3","-","0",".","+","="]

	function appendToDisplay(event) {
		const numbers = "0123456789.";
		const signs = "+-*/";
		const allowedKeys = "0123456789+-*/()=.";
		
		if (event instanceof KeyboardEvent){
			console.log(event)
			event.preventDefault();
		}

 		if (event instanceof KeyboardEvent && allowedKeys.includes(event.key) ||  event.key === "Backspace" || event.key === "Enter") {
			event = event.key;
		} else if (event === "x") {
			event = "*";
		} 

		if (numbers.includes(event) || event === "(" || event === ")") {
			displayed = (equalstate === 1 ? event :  displayed.toString() + event.toString());
			equalstate = 0;
		} else if (signs.includes(event)) {
			if (signs.includes(displayed[displayed.length-1])){
				displayed = displayed.slice(0, -1) + event;
			} else{
				displayed = displayed + event;
				equalstate = 0;
			}
		} else if (event === "CE"){
			displayed = "";
			equalstate = 0;
		} else if (event === "C" || event === "Backspace"){
			displayed = (equalstate === 1 ? "" :  displayed.slice(0, -1));
			equalstate = 0;
		} else if (event === "=" || event === "Enter"){
			displayed = eval(displayed);
			equalstate = 1;
		} 
		return(displayed)
	}
</script>

<div class="box outer">
	<input class="box inputbox" style="color:black" bind:value={displayed} onkeydown={appendToDisplay}>	
	{#each buttons as button}
		<button onclick={() => appendToDisplay(button) } style="font-weight: bold" style:background-color={(button === "=" ? 'orange' : 'lightgray')}>{button}</button>
	{/each}
</div>
<style>
	.box {
		border: 2px solid black;
		border-radius: 0.5em;
	}

	button {
		width:50px;
		height:50px;
		border: 1px solid black;
		position: relative;
		top: 20px;
		margin: 2.5px;
		border-radius: 6px
	}

	.outer {
		width: 250px;
		height: 380px;
		background-color: darkgray;
		text-align: center;
		margin: 100px;
	}

	.inputbox {
		width: 225px;
		height: 40px;
		margin-top: 15px;
		font-size:25px;
		background-color: white;
		text-align: right;
		padding: 5px 5px;
		position: relative;
	}
</style>
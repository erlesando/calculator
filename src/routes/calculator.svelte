<script>
	let displayed = $state("");
	let equalstate = $state(0);

	function appendToDisplay(event) {
		const numbers = "0123456789";
		const signs = "+-*/";
		const allowedKeys = "0123456789+-*/()=";

		if (event instanceof KeyboardEvent && !allowedKeys.includes(event.key) && event.key !== "Backspace" && event.key !== "Enter") {
			event.preventDefault();
		} else if (numbers.includes(event)) {
			if (equalstate === 1){
				displayed = event;
				equalstate = 0;
			} else {
				displayed = displayed.toString() + event.toString();
				equalstate = 0;
			}
		} else if (signs.includes(event)) {
			if (signs.includes(displayed[displayed.length-1])){
				displayed = displayed.slice(0, -1);
				displayed = displayed + event;
			} else{
				displayed = displayed + event;
				equalstate = 0;
			}
		} else if (event === "CE"){
			displayed = "";
			equalstate = 0;
		} else if (event === "C"){
			displayed = displayed.slice(0, -1);
			equalstate = 0;
		} else if (event === "=" || event.key === "Enter"){
			displayed = eval(displayed);
			equalstate = 1;
		} else {
			displayed = displayed + event;
			equalstate = 0;
		}
		return(displayed)
	}
</script>

<div class=box id="outer">
	<input class=box id="visual" style="color:black" bind:value={displayed} onkeydown={appendToDisplay}>
	<button class="num" onclick={() => appendToDisplay("CE")}>CE</button>
	<button class="num" onclick={() => appendToDisplay("C")}>C</button>
	<button class="num" onclick={() => appendToDisplay("(")}>(</button>
	<button class="num" onclick={() => appendToDisplay(")")}>)</button>
	<button class="num" onclick={() => appendToDisplay(7)}>7</button>
	<button class="num" onclick={() => appendToDisplay(8)}>8</button>
	<button class="num" onclick={() => appendToDisplay(9)}>9</button>
	<button class="num" onclick={() => appendToDisplay("/")}>/</button>
	<button class="num" onclick={() => appendToDisplay(4)}>4</button>
	<button class="num" onclick={() => appendToDisplay(5)}>5</button>
	<button class="num" onclick={() => appendToDisplay(6)}>6</button>
	<button class="num" onclick={() => appendToDisplay("*")}>x</button>
	<button class="num" onclick={() => appendToDisplay(1)}>1</button>
	<button class="num" onclick={() => appendToDisplay(2)}>2</button>
	<button class="num" onclick={() => appendToDisplay(3)}>3</button>
	<button class="num" onclick={() => appendToDisplay("-")}>-</button>
	<button class="num" onclick={() => appendToDisplay(0)}>0</button>
	<button class="num" onclick={() => appendToDisplay(".")} style="font-weight: bold">.</button>
	<button class="num" onclick={() => appendToDisplay("+")}>+</button>
	<button class="num" style="background-color:orange"
		onclick={() => appendToDisplay("=")}>=</button>
</div>
<style>
	.box {
		border: 2px solid black;
		border-radius: 0.5em;
	}

	.num {
		width:50px;
		height:50px;
		border: 1px solid black;
		position: relative;
		top: 20px;
		margin: 2.5px;
		border-radius: 6px
	}

	#outer {
		width: 250px;
		height: 380px;
		background-color: darkgray;
		text-align: center;
		margin: 100px;
	}

	#visual {
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
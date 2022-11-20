<template>
	<div>
		<div v-if='step===1'>
			<p>List your factions, separated with commas (eg : faction1,faction2)</p>
			<textarea cols="20" rows="10" v-model="factionString"></textarea>
			<p>Input your territory list that you got from the triplea official mod tools</p>
			<textarea cols="100" rows="50" v-model="DataText"></textarea>
			<button @click="stepOne">Next step</button>
		</div>
		<div v-if='step===2'>
			<div  v-for='(line,index) in this.finalObj'>
				<p>{{line.name}}</p>
				<div class='owners'>
					<label>
						<input type="radio" value="" :name="line.name" v-model="line.owner" checked><span :for="line.name">Neutrals</span>
					</label>
					<label v-for="faction in factions">
						<input type="radio" :value="faction" :name='line.name' v-model="line.owner"><span :for="line.name">{{faction}}</span>
					</label>
					
				</div>
				<p>-------</p>
			</div>
			<button @click='doneMethod()'>Done!</button>

		</div>
		<div v-if='step===3'>
			<div  v-for='(line,index) in this.attachments'>
				<p v-for='(item,index) in line.split("\n")'>{{item}}</p>
			</div>
		</div>
		
	</div>
</template>

<script>
export default {
  name: 'IndexPage',
  fetch() {
  	
  },

  methods: {
  	logObj(index) {
  		console.log('finalObj['+index+']',this.finalObj[index])
  	},
  	stepOne() {
  		console.log(this.DataText)
	  	this.DataArray = this.DataText.split('\n');
	  	console.log(this.DataArray[0])
	  	this.factions=this.factionString.split(',')

	  	this.DataArray.forEach((line,index) => {
	  	console.log(line)
	  		let lineSplit = line.split(' ')
	  		this.finalObj[index] = {
	  			name : lineSplit[1].split('"')[1],
	  			owner : ''
	  		}
	  	})
	  	console.log('finalObj',this.finalObj)
	  	this.step=2
  	},
  	doneMethod() {
  		let ownerInitialize = []
  		Object.entries(this.finalObj).forEach(item => {
  			let line = item[1]
  			console.log(line)
  			if (line.owner!=="") {
  				ownerInitialize.push('<territoryOwner territory="'+line.name+'" owner="'+line.owner+'"/>\ ')
  			} 
  		})
  		console.log(ownerInitialize)
		this.step=3
		this.attachments=ownerInitialize
  	}
  },

  data() {
  	return {
  		DataArray: [],
  		finalObj: {},
  		step: 1,
  		attachments:[],
  		factionString : '',
  		factions: [
  
  		],
  		DataText: 
  		``
  	}
  }
}
</script>

<style scoped>
	.owners {
		display: flex;
		flex-direction: column;
	}
	label {
		background-color: silver;
		padding:5px;
		margin-bottom: 2px
	}
</style>
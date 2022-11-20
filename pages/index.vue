<template>
	<div>
		<div v-if='!done'>
			<div  v-for='(line,index) in this.finalObj'>
				<p>{{line.name}}</p>
				<label>Production Value : </label>
				<input type='number' v-model='line.production'></input>
				<button v-on:click='logObj(index)'>rzgrs</button>	
				<input type="checkbox" id="impassable" v-model='line.isImpassable'>Impassable</input>
				<input type="checkbox" id="victoryCity" v-model='line.victoryCity'>Victory city</input>
				<input type="checkbox" id="capital" v-model='line.capital.status'>Capital</input>
				<input type="text" name="capital" v-if='line.capital.status===true' v-model='line.capital.value' placeholder="Owner"></input>
				<p>-------</p>
			</div>
			<button @click='doneMethod()'>Done!</button>

		</div>
		<div v-else>
			<div  v-for='(line,index) in this.attachments'>
				<div v-for='(item,index) in line.split("\n")'>{{item}}</div>
				<br>
			</div>
		</div>
		
	</div>
</template>

<script>
export default {
  name: 'IndexPage',
  fetch() {
  	console.log(this.DataText)
  	this.DataArray = this.DataText.split('\n');
  	console.log(this.DataArray[0])

  	this.DataArray.forEach((line,index) => {
  	console.log(line)
  		let lineSplit = line.split(' ')
  		this.finalObj[index] = {
  			name : lineSplit[1].split('"')[1],
  			production : 2,
  			victoryCity: false,
  			isImpassable: false,
  			capital: {
  				status: false,
  				value: ''
  			}
  		}
  	})
  	console.log('finalObj',this.finalObj)
  },

  methods: {
  	logObj(index) {
  		console.log('finalObj['+index+']',this.finalObj[index])
  	},
  	doneMethod() {
  		let attachments = []
  		Object.entries(this.finalObj).forEach(item => {
  			let line = item[1]
  			console.log(line)
  			let attachmentToPush = '<attachment name="territoryAttachment" attachTo="'+line.name+'" javaClass="games.strategy.triplea.attachments.TerritoryAttachment" type="territory">\
        				\n<option name="production" value="'+line.production+'"/>\ '
  			if (line.capital.status===true) {
  				attachmentToPush= attachmentToPush.concat(
  					'\n<option name="capital" value="'+line.capital.value+'"/>\ '
				)	
  			} 
  			if (line.victoryCity===true) {
  				attachmentToPush= attachmentToPush.concat(
  					'\n<option name="victoryCity" value="1"/>\ '
				)	
  			} 
  			if (line.isImpassable===true) {
  				attachmentToPush= attachmentToPush.concat(
  					'\n<option name="isImpassable" value="true"/>\ '
				)	
  			} 
  			attachmentToPush= attachmentToPush.concat(
  					'\n</attachment>'
				)	
  			attachments.push(attachmentToPush)
  		})
  		console.log(attachments)
  			this.done=true
  			this.attachments=attachments
  	}
  },

  data() {
  	return {
  		DataArray: [],
  		finalObj: {},
  		done: false,
  		attachments:[],
  		DataText: 
  		` INSERT YOUR TERRITORIES HERE IN THIS FORMAT :
  		<territory name="Alpha_moon_1"/>
		<territory name="Alpha_moon_2"/>`
  	}
  }
}
</script>

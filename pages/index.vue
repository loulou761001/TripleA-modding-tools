<template>
	<div>
		<div v-if='step===1'>
			<p>Input your territory list that you got from the triplea official mod tools</p>
			<textarea cols="100" rows="50" v-model="DataText"></textarea>
			<button @click="stepOne">Next step</button>
		</div>
		<div v-if='step===2'>
			<div  v-for='(line,index) in this.finalObj'>
				<p>{{line.name}}</p>
				<label>Production Value : </label>
				<input type='number' v-model='line.production'></input>
				<input type="checkbox" id="impassable" v-model='line.isImpassable'>Impassable</input>
				<input type="checkbox" id="victoryCity" v-model='line.victoryCity'>Victory city</input>
				<input type="checkbox" id="capital" v-model='line.capital.status'>Capital</input>
				<input type="text" name="capital" v-model='line.capital.value' placeholder="Capital owner (ONLY WORKS IF CAPITAL IS CHECKED)"></input>
				<p>-------</p>
			</div>
			<button @click='doneMethod()'>Done!</button>

		</div>
		<div v-if='step===3'>
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
  	
  },
  watch: {
  	finalObj() {
  		console.log(this.finalObj)
  	}
  },
  methods: {
  	changeCapStatus(index,data) {
  		console.log('change')
  		if(this.finalObj[index].capital.status===true) {
  			this.finalObj[index].capital.status=false
  		} else {
  			this.finalObj[index].capital.status=true
  		}
  		console.log(this.finalObj[index].capital.status)
  		console.log(data)
  	},
  	logObj(index) {
  		console.log('finalObj['+index+']',this.finalObj[index])
  	},
  	async stepOne() {
	  	this.DataArray = this.DataText.split('\n');
	  	console.log(this.DataArray[0])

	  	await this.DataArray.forEach((line,index) => {
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
	this.step=2
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
		this.step=3
		this.attachments=attachments
  	}
  },

  data() {
  	return {
  		DataArray: [],
  		finalObj: {},
  		step: 1,
  		attachments:[],
  		DataText: 
  		``
  	}
  }
}
</script>

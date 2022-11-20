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
  		`<territory name="Alpha_moon_1"/>
		<territory name="Alpha_moon_2"/>
		<territory name="Alpha_way"/>
		<territory name="Aquilonis_1"/>
		<territory name="Aquilonis_2"/>
		<territory name="Aquilonis_3"/>
		<territory name="Aquilonis_4"/>
		<territory name="Aquilonis_5"/>
		<territory name="Aquilonis_6"/>
		<territory name="Aquilonis_7"/>
		<territory name="Aquilonis_8"/>
		<territory name="Aquilonis_9"/>
		<territory name="Aquilonis_10"/>
		<territory name="Aquilonis_11"/>
		<territory name="Aquilonis_12"/>
		<territory name="Aquilonis_13"/>
		<territory name="Aquilonis_14"/>
		<territory name="Aquilonis_15"/>
		<territory name="Aquilonis_16"/>
		<territory name="Aquilonis_17"/>
		<territory name="Aquilonis_18"/>
		<territory name="Aquilonis_19"/>
		<territory name="Aquilonis_20"/>
		<territory name="Aquilonis_21"/>
		<territory name="Aquilonis_22"/>
		<territory name="Aquilonis_23"/>
		<territory name="Aquilonis_24"/>
		<territory name="Beta_moon_1"/>
		<territory name="Beta_moon_2"/>
		<territory name="Beta_way"/>
		<territory name="Gamma_moon_1"/>
		<territory name="Gamma_moon_2"/>
		<territory name="Gamma_way"/>
		<territory name="Mare_sanguinis_1"/>
		<territory name="Mare_sanguinis_2"/>
		<territory name="Mediolanum_1"/>
		<territory name="Mediolanum_2"/>
		<territory name="Mediolanum_3"/>
		<territory name="Mediolanum_4"/>
		<territory name="Mediolanum_5"/>
		<territory name="Mediolanum_6"/>
		<territory name="Mediolanum_7"/>
		<territory name="Mediolanum_8"/>
		<territory name="Mediolanum_9"/>
		<territory name="Mediolanum_10"/>
		<territory name="Mediolanum_11"/>
		<territory name="Mediolanum_12"/>
		<territory name="Mediolanum_13"/>
		<territory name="Mediolanum_14"/>
		<territory name="Mediolanum_15"/>
		<territory name="Mediolanum_16"/>
		<territory name="Mediolanum_17"/>
		<territory name="Mediolanum_19"/>
		<territory name="Mediolanum_20"/>
		<territory name="Mediolanum_21"/>
		<territory name="Mediolanum_22"/>
		<territory name="Mediolanum_23"/>
		<territory name="Mediolanum_24"/>
		<territory name="Mediolanum_25"/>
		<territory name="Meridiem_1"/>
		<territory name="Meridiem_2"/>
		<territory name="Meridiem_3"/>
		<territory name="Meridiem_4"/>
		<territory name="Meridiem_5"/>
		<territory name="Meridiem_6"/>
		<territory name="Meridiem_7"/>
		<territory name="Meridiem_8"/>
		<territory name="Meridiem_9"/>
		<territory name="Meridiem_10"/>
		<territory name="Meridiem_11"/>
		<territory name="Meridiem_12"/>
		<territory name="Meridiem_13"/>
		<territory name="Meridiem_14"/>
		<territory name="Meridiem_15"/>
		<territory name="Meridiem_16"/>
		<territory name="Meridiem_17"/>
		<territory name="Meridiem_18"/>
		<territory name="Meridiem_19"/>
		<territory name="Meridiem_20"/>
		<territory name="Mountainous_seas"/>
		<territory name="Occidens_1"/>
		<territory name="Occidens_2"/>
		<territory name="Occidens_3"/>
		<territory name="Occidens_4"/>
		<territory name="Occidens_5"/>
		<territory name="Occidens_6"/>
		<territory name="Occidens_7"/>
		<territory name="Occidens_8"/>
		<territory name="Occidens_9"/>
		<territory name="Occidens_10"/>
		<territory name="Occidens_11"/>
		<territory name="Occidens_12"/>
		<territory name="Occidens_13"/>
		<territory name="Occidens_14"/>
		<territory name="Occidens_15"/>
		<territory name="Occidens_16"/>
		<territory name="Occidens_17"/>
		<territory name="Occidens_18"/>
		<territory name="Occidens_19"/>
		<territory name="Orientis_1"/>
		<territory name="Orientis_2"/>
		<territory name="Orientis_3"/>
		<territory name="Orientis_4"/>
		<territory name="Orientis_5"/>
		<territory name="Orientis_6"/>
		<territory name="Orientis_7"/>
		<territory name="Orientis_8"/>
		<territory name="Orientis_9"/>
		<territory name="Orientis_10"/>
		<territory name="Orientis_11"/>
		<territory name="Orientis_12"/>
		<territory name="Orientis_13"/>
		<territory name="Orientis_14"/>
		<territory name="Orientis_15"/>
		<territory name="Space_1" water="true"/>
		<territory name="Space_2" water="true"/>
		<territory name="Space_3" water="true"/>
		<territory name="Space_4" water="true"/>`
  	}
  }
}
</script>

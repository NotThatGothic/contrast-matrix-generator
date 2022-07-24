<template>
	<!-- Color data -->
	<div class="col-12">
		<h2>Color data </h2>
		<form class="form row d-flex align-items-end" id="load-form" @submit.prevent  spellcheck="false">
			<div class="col">
				<textarea id="color-data" class="form-control" style="max-height: 100em; min-height: 6em;" :style="'height:' + textareaHeight + 'em'" type="text" name="color-data" v-model="colorDataString"/>
			</div>
			<div class="col-auto">
				<div class="d-flex flex-column">
					<button class="btn btn-primary mb-3" type="btn" v-on:click="saveColorData()">Save Color Data</button>
					<button class="btn btn-primary" type="btn" v-on:click="loadColorData()">Load Color Data</button>
				</div>
			</div>
		</form>
	</div>
</template>

<script>
	export default {
	name: 'ColorSave',
	props: {
		data: Array
	},
	data() {
		return {
			textareaHeight: 6,
			colorDataString: ''
		}
	},
	methods: {
		saveColorData() {
			this.colorDataString = JSON.stringify(this.data)
			this.textareaHeight = (this.data.length  * 1.75)
		},
		loadColorData() {
			if (this.colorDataString == false) {
				alert('Enter some JSON data to load')
			} else {
				if (validate(this.colorDataString)) {
					this.$emit('loadData', this.colorDataString)
					this.colorDataString = ""
					this.textareaHeight = 6
				} else {
					alert('Invalid JSON')
				}
			}
		}
	}
}
function validate(str) {
    try {
        JSON.parse(str);
    } catch (e) {
        return false;
    }
    return true;
}
</script>

<style scoped lang="stylus">
	textarea {font-family: monospace; height: 10em}
</style>
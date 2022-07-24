<template>
	<!-- Add colors -->
	<div class="col-auto mb-3">
		<h2>Add colors</h2>
		<form class="form row d-flex align-items-end" id="color-form" @submit.prevent="addColor" autocomplete="off">
			<div class="col-auto">
				<label for="new-color-name" class="form-label">Name </label>
				<input id="new-color-name" class="form-control" type="text" name="new-color-name" v-model="newColorName" required />
			</div>
			<div class="col-auto">
				<label for="new-color-hex" class="form-label">Hexcode </label>
				<input id="new-color-hex" class="form-control" type="text" name="new-color-hex" v-model="newColorHex" required />
			</div>
			<div class="col-auto">
				<button class="btn btn-primary mt-3" type="submit" form="color-form">Add Color</button>
			</div>
		</form>
	</div>
</template>

<script>
	export default {
	name: 'AddColors',
	data() {
		return {
			newColorName: '',
			newColorHex: ''
		}
	},
	methods: {
		addColor() {
			if (this.newColorName == false || this.newColorHex == false) {
				alert("Name and Hex are mandatory")
			} else {
				this.$emit('addColor', this.newColorName, this.newColorHex)
			}
		}
	}
}
</script>
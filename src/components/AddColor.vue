<template>
	<!-- Add colors -->
	<div class="col-12">
		<h2>Add colors</h2>
		<form class="form row d-flex align-items-end" id="color-form" @submit.prevent="addColor" autocomplete="off">
			<div class="col-12 col-md-auto mb-3 mb-md-0">
				<label for="new-color-name" class="form-label">Name </label>
				<input id="new-color-name" class="form-control" type="text" name="new-color-name" v-model="newColorName"
					required />
			</div>
			<div class="col-12 col-md-auto mb-3 mb-md-0">
				<div class="row d-flex align-items-end">
					<div class="col">
						<label for="new-color-code" class="form-label">Color</label>
						<input id="new-color-code" class="form-control" type="text" name="new-color-code"
						placeholder="Hex or RGB" v-model="newColorCode" required />
					</div>
				</div>
			</div>
			<div class="col-auto mb-3 mb-md-0">
				<button class="btn btn-primary" type="submit" form="color-form">Add Color</button>
			</div>
		</form>
	</div>
</template>

<script>
import chroma from "chroma-js";

export default {
	name: 'AddColors',
	data() {
		return {
			newColorName: '',
			newColorCode: '',
		}
	},
	methods: {
		addColor() {
			if (this.newColorName == false || this.newColorCode === false) {
				alert("Name and Color value are mandatory")
			} else {
				if ((/(?:rgb)?:?\s?\(?(\d{1,3})(?:\s|,|,\s)(\d{1,3})(?:\s|,|,\s)(\d{1,3})\)?/gi).test(this.newColorCode)) {
					this.newColorCode = RGBToHex(standardizeRGB(this.newColorCode))
					this.$emit('addColor', this.newColorName, this.newColorCode)
					this.newColorName = ''
					this.newColorCode = ''
				} else {
					alert("Please input a valid Hexcode or RGB value")
				}
			}
		},
	}
}
</script>

if (this.newColorName == false || this.newColorCode === false) {
	alert("Name and Color value are mandatory")
} else {
	if (chroma.valid(this.newColorCode) == true ) {
		this.newColorName = new Color(this.newColorCode);
	} else {
		alert("This color code is not valid.")
	}
}
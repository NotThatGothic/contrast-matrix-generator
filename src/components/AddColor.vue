<template>
	<!-- Add colors -->
	<div class="col-auto mb-3">
		<h2>Add colors</h2>
		<form class="form row d-flex align-items-end" id="color-form" @submit.prevent="addColor" autocomplete="off">
			<div class="col-auto">
				<label for="new-color-name" class="form-label">Name </label>
				<input id="new-color-name" class="form-control" type="text" name="new-color-name" v-model="newColorName"
					required />
			</div>
			<div class="col-auto">
				<label for="new-color-code" class="form-label">Color</label>
				<input id="new-color-code" class="form-control" type="text" name="new-color-code"
					placeholder="Hex or RGB" v-model="newColorCode" required />
			</div>
			<div class="col-auto">
				<input id="new-color-code" class="form-control form-control-color" type="color" name="new-color-code" v-model="newColorCode" />
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
			newColorCode: ''
		}
	},
	methods: {
		addColor() {
			if (this.newColorName == false || this.newColorCode === false) {
				alert("Name and Color value are mandatory")
			} else {
				if ((/^#?([a-f\d])([a-f\d])([a-f\d])$|^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i).test(this.newColorCode)) {
					this.$emit('addColor', this.newColorName, this.newColorCode)
					this.newColorName = ''
					this.newColorCode = ''
				} else if ((/\(?\d+[,\s]?\d+[,\s]?\d+\)?/).test(this.newColorCode)) {
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
function standardizeRGB(value) {
	var regex = /RGB:? ?|\(|\)/ig
	return value = value.replaceAll(regex, '')
}

function RGBToHex(rgb) {
	var regex = /((\d+)[,\s]?\s?){2}(\d+)/
	rgb = rgb.replace(regex, function(m, r,g,b) {
		r = Math.abs(r).toString(16)
		g = Math.abs(g).toString(16)
		b = Math.abs(b).toString(16)
		if (r.length == 1) {
			r = '0' + r
		}
		if (g.length == 1) {
			g = '0' + g
		}
		if (b.length == 1) {
			b = '0' + b
		}
		return ('#' + r + g + b)
	})
	return rgb
}
</script>
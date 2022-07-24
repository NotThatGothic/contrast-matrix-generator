<template>
	<tr>
		<td class="align-middle">{{ id }}</td>
		<th class="align-middle">
			<form class="form" autocomplete="off" @submit.prevent="updateName(rowName)">
				<input :id="id + 'name'" class="form-control border-0 bg-transparent shadow-none w-auto" type="text" name="color-color-name" v-model="rowName" required />
			</form>
		</th>
		<td class="align-middle text-center" v-if="displayOptions.showSample" ><span style="font-size: 5ex; line-height: 1ex;" :style="{ color: '#' + rowHexCalc }">■</span></td>
		<td class="align-middle">
			<form class="form" autocomplete="off" @submit.prevent="updateHex(rowHex)">
				<input :id="id + 'colorSpan'" class="form-control border-0 bg-transparent shadow-none w-auto" type="text" name="color-color-name" v-model.lazy="rowHex" required />
			</form>
		</td>
		<td class="align-middle">RGB: ({{ rgb.r }}, {{ rgb.g }}, {{ rgb.b }})</td>
		<td class="align-middle" v-if="displayOptions.showLum" >Lum: {{ lum }}</td>
		<td class="align-middle text-end">
			<button type="button" class="close btn btn-danger btn-sm" aria-label="Close"  @click="removeRow()">
				<span aria-hidden="true">&times;</span>
			</button>
		</td>
	</tr>
</template>

<script>
export default {
	name: 'ColorRow',
	props: {
		id: Number, 
		name: String,
		hex: String,
		displayOptions: Object,
	},
	data() {
		return {
			rowName: this.name,
			rowHex: '#' + this.hex,
			
		}
	},
	computed: {
		rgb() {
			return hexToRGB(this.rowHex)
		},
		rowHexCalc() {
			return this.rowHex.substring(1, 7);
		},
		lum() {
			return calculateLum(hexToRGB(this.rowHex).r, hexToRGB(this.rowHex).g, hexToRGB(this.rowHex).b)
		}
	},
	methods: {
		updateName(name) {
			this.rowName = name
			this.$emit('updateName', this.rowName, this.id)
			// console.log(this.rowName)
		},
		updateHex(hex) {
			this.rowHex = standardizeHex(hex)
			this.$emit('updateHex', this.rowHex, this.id)
			// console.log(this.rowHex)
		},
		removeRow() {
			this.$emit('removeRow', this.id)
			// console.log(this.rowHex)
		}
	}
}

function standardizeHex(value) {
	if (value.startsWith("#")) {
		value = value.substring(1, 7);
	}
	if (value.length == 3) {
		value = value[0] + value[0] + value[1] + value[1] + value[2] + value[2];
	}
	return value;
}

function hexToRGB(hex) {
	var shorthandRegex = /^#?([a-f\d])([a-f\d])([a-f\d])$/i;
	hex = hex.replace(shorthandRegex, function(m, r, g, b) {
		return r + r + g + g + b + b;
	});

	var result = /^#?([a-f\d]{2})([a-f\d]{2})([a-f\d]{2})$/i.exec(hex);
	return result ? {
		r: parseInt(result[1], 16),
		g: parseInt(result[2], 16),
		b: parseInt(result[3], 16)
	} : null;
}

function calculateLum(r, g, b) {
	var a = [r, g, b].map(function (v) {
        v /= 255;
        return v <= 0.03928
            ? v / 12.92
            : Math.pow( (v + 0.055) / 1.055, 2.4 );
    });
    return a[0] * 0.2126 + a[1] * 0.7152 + a[2] * 0.0722;
}
</script>

<style scoped lang="stylus">
form:hover,
form:focus-within {
	text-decoration: underline;
	text-decoration-color: var(--bs-primary);
 }
</style>
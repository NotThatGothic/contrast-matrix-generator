<template>
	<div v-if="displayOptions.palette" class="text-center p-2 border">
		<template v-if="!displayOptions.readOnly">
			<form class="form" autocomplete="off" @submit.prevent="updateName(rowName)">
				<input style="width: 150px" :id="id + 'name'" class="text-center form-control" type="text" name="color-color-name" v-model="rowName" required />
			</form>
		</template>
		<template v-else>
			<p class="form-control">{{rowName}}</p>
		</template>
		<template v-if="!displayOptions.readOnly">
			<form class="form" autocomplete="off" @submit.prevent="updateHex(rowHex)">
				<input style="width: 150px" :id="id + 'colorHex'" class="text-center form-control text-muted" type="text" name="color-color-hex" v-model="rowHex" required />
			</form>
		</template>
		<template v-else>
			<p class="form-control text-muted">{{rowHex}}</p>
		</template>
		<template v-if="displayOptions.showRGB">
			<template v-if="!displayOptions.readOnly">
				<form class="form" autocomplete="off" @submit.prevent="updateRGB(rowRGB)">
					<input style="width: 150px" :id="id + 'colorRGB'" class="text-center form-control text-muted" type="text" name="color-color-hex" v-model="rowRGB" required />
				</form>
			</template>
			<template v-else>
				<p class="form-control text-muted">{{rowRGB}}</p>
			</template>
		</template>
		<template v-if="displayOptions.showLum">
			<p class="form-control">{{lum}}</p>
		</template>
		<div v-if="displayOptions.showSample" style="top: -50px; position: relative; margin-bottom: -50px; z-index: -1;">
			<p  class="form-control" style="font-size: 150px; line-height:1" :style="{ color: rowHex }">■</p>
		</div>
		<button v-if="!displayOptions.readOnly" type="button" class="close btn btn-danger btn-sm" aria-label="Close"  @click="removeRow()">
			<span aria-hidden="true">&times;</span>
		</button>
	</div>
	<tr v-else>
		<th class="align-middle">
			<template v-if="!displayOptions.readOnly">
				<form class="form" autocomplete="off" @submit.prevent="updateName(rowName)">
					<input :id="id + 'name'" class="form-control" type="text" name="color-color-name" v-model="rowName" required />
				</form>
			</template>
			<template v-else>
				<p class="form-control fw-normal"> {{rowName}} </p>
			</template>
		</th>
		<td class="align-middle text-center" v-if="displayOptions.showSample" ><span style="font-size: 5ex; line-height: 1ex;" :style="{ color: rowHex }">■</span></td>
		<td class="align-middle">
			<template v-if="!displayOptions.readOnly">
				<form class="form" autocomplete="off" @submit.prevent="updateHex(rowHex)">
					<input :id="id + 'colorHex'" class="form-control text-muted" type="text" name="color-color-hex" v-model="rowHex" required />
				</form>
			</template>
			<template v-else>
				<p class="form-control text-muted"> {{rowHex}} </p>
			</template>
		</td>
		<td v-if="displayOptions.showRGB" class="align-middle">
			<template v-if="!displayOptions.readOnly">
				<form class="form" autocomplete="off" @submit.prevent="updateRGB(rowRGB)">
					<input :id="id + 'colorRGB'" class="form-control text-muted" type="text" name="color-color-rgb" v-model="rowRGB" required />
				</form>
			</template>
			<template v-else>
				<p class="form-control text-muted"> {{rowRGB}} </p>
			</template>
		</td>
		<td class="align-middle" v-if="displayOptions.showLum">Lum: {{ lum }}</td>
		<td class="align-middle text-end" v-if="!displayOptions.readOnly">
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
		chroma: String,
		displayOptions: Object,
	},
	data() {
		return {
			rowName: this.name,
			rowHex: '#' + this.hex,
			rowRGB: 'RGB: (' + hexToRGB(this.hex).r + ',' + hexToRGB(this.hex).g + ',' + hexToRGB(this.hex).b + ')'
		}
	},
	computed: {
		rgb() {
			return hexToRGB(this.rowHex)
		},
		lum() {
			return calculateLum(hexToRGB(this.rowHex).r, hexToRGB(this.rowHex).g, hexToRGB(this.rowHex).b)
		}
	},
	methods: {
		updateName(name) {
			this.rowName = name
			this.$emit('updateName', this.rowName, this.id)
		},
		updateHex(hex) {
			this.rowHex = standardizeHex(hex)
			this.rowRGB = 'RGB: (' + this.rgb.r + ',' + this.rgb.g + ',' + this.rgb.b + ')'
			this.$emit('updateHex', this.rowHex, this.id)
		},
		updateRGB(rgb) {
			this.rowRGB = 'RGB: (' + standardizeRGB(rgb) + ')'
			this.rowHex = RGBToHex(standardizeRGB(rgb))
			this.$emit('updateHex', this.rowHex, this.id)
		},
		removeRow() {
			this.$emit('removeRow', this.id)
		}
	}
}

function standardizeHex(value) {
	if (value.length == 3) {
		value = '#' + value[0] + value[0] + value[1] + value[1] + value[2] + value[2];
	}
	if (value.length == 4) {
		value = value[0] + value[1] + value[1] + value[2] + value[2] + value[3] + value[3];
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

function standardizeRGB(value) {
	var regex = /rgb:?|\(|\)|,(?= )/ig
	value = value.replaceAll(regex, '')
	var regex2 = /,/g
	return value = value.replaceAll(regex2, ' ')
}

function RGBToHex(rgb) {
	var regex = /^(\d+)\s(\d+)\s(\d+)$/
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

<style scoped lang="stylus">
form:hover,
form:focus-within {
	text-decoration: underline;
	text-decoration-color: var(--bs-primary);
 }
 .form-control {
	border: none;
	background: transparent;
	padding: 0;
	margin-bottom: 0;
	box-shadow: none;
 }
</style>
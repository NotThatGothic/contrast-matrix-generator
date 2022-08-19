<template>
	<!-- Add color -->
    <div class="row justify-content-between">
		<AddColor @add-color="addColor" />
			
		<div class="col-auto d-flex flex-column justify-content-between mb-3">
            <h2>Display Options</h2>
			<form class="form" @submit.prevent>
				<div class="form-check">
					<input class="form-check-input" type="checkbox" value="" id="showSample" v-model="displayOptions.showSample">
					<label class="form-check-label" for="showSample">
						Show Sample
					</label>
				</div>
				<div class="form-check">
					<input class="form-check-input" type="checkbox" value="" id="showLum" v-model="displayOptions.showLum">
					<label class="form-check-label" for="showLum">
						Show Luminosity
					</label>
				</div>
				<div class="form-check">
					<input class="form-check-input" type="checkbox" value="" id="showHex" v-model="displayOptions.showHex">
					<label class="form-check-label" for="showHex">
						Show Hex on Table
					</label>
				</div>
			</form>
		</div>
	</div>
	<!-- Current colors -->
    <div class="row mb-3">
        <div class="col-12">
            <h2>Current colors</h2>
			<div style="overflow-x: auto;" v-if="colors.length > 0">
				<table class="table table-hover border">
					<thead>
						<tr>
							<th style="width:5ex">ID</th>
							<th style="width:auto">Color name</th>
							<th style="width:20ex" class="text-center" v-if="displayOptions.showSample">Color sample</th>
							<th style="width:auto">Color hexcode</th>
							<th style="width:auto">Color RGB value</th>
							<th style="width:auto" v-if="displayOptions.showLum">Color Lum</th>
							<th></th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(color) in colors" :key="color.id" is="vue:ColorRow" :id="color.id" :name="color.colorName" :hex="color.colorHex" :displayOptions="displayOptions" @update-name="updateName" @update-hex="updateHex" @remove-row="removeRow" />
					</tbody>
					<tfoot>
						<tr>
							<td colspan="100%" class="align-middle text-end">
								<button type="button" class="close btn btn-danger" aria-label="Close"  @click="colors = []">
									<span aria-hidden="true">Delete All</span>
								</button>
							</td>
						</tr>
					</tfoot>
				</table>
			</div>
			<div v-else>
				<p class="text-muted">Add some colors with the form, import colors from JSON below or <a href="#" @click="defaultData">load some example colors from Bootstrap</a>.</p>
			</div>
        </div>
    </div>
	<!-- Contrast table -->
    <div class="row mb-3">
        <div class="col-12">
			<div class="row justify-content-between">
				<div class="col-6 mb-3">
					<h2>Contrast table</h2>
				</div>
				<div class="col-4 mb-3">
					<div class="accordion" id="accordionOptions">
						<div class="accordion-item">
							<h2 class="accordion-header" id="optionsHeading">
								<button class="accordion-button collapsed me-2" type="button" data-bs-toggle="collapse" data-bs-target="#optionsCollapse" aria-expanded="false" aria-controls="optionsCollapse">
									Contrast table options
								</button>
							</h2>
							<div id="optionsCollapse" class="accordion-collapse collapse" aria-labelledby="optionsHeading" data-bs-parent="#accordionOptions">
								<div class="accordion-body">
									<form class="form row" @submit.prevent >
										<div class="col-auto mb-3">
											<div class="form-check">
												<input class="form-check-input" type="radio" value="AAA" id="AAA" v-model="wcagLevel">
												<label class="form-check-label" for="AAA">
													Success Level AAA
												</label>
											</div>
											<div class="form-check">
												<input class="form-check-input" type="radio" value="AA" id="AA" v-model="wcagLevel">
												<label class="form-check-label" for="AA">
													Success Level AA
												</label>
											</div>
											<div class="form-check">
												<input class="form-check-input" type="radio" value="A" id="A" v-model="wcagLevel">
												<label class="form-check-label" for="A">
													Success Level A
												</label>
											</div>
										</div>
										<div class="col-auto mb-3">
											<div class="form-check">
												<input class="form-check-input" type="radio" value="defaultResults" id="defaultResults" v-model="resultsRendering">
												<label class="form-check-label" for="defaultResults">
													Default results
												</label>
											</div>
											<div class="form-check">
												<input class="form-check-input" type="radio" value="colorizeResults" id="colorizeResults" v-model="resultsRendering" checked>
												<label class="form-check-label" for="colorizeResults">
													Colorize results
												</label>
											</div>
											<div class="form-check">
												<input class="form-check-input" type="radio" value="renderResults" id="renderResults" v-model="resultsRendering">
												<label class="form-check-label" for="renderResults">
													Render results
												</label>
											</div>
										</div>
										<div class="col-auto">
											<div v-if="resultsRendering == 'renderResults'">
												<label for="example-text" class="form-label">Example Text</label>
												<input id="example-text" class="form-control" type="text" name="example-text" v-model="exampleText" />
											</div>
										</div>
									</form>
								</div>
							</div>
						</div>
					</div>
				</div>
			</div>
			<div style="overflow-x:auto;" v-if="colors.length > 1">
				<table id="contrast-matrix-table" class="table table-bordered table-assist">
					<thead>
						<tr>
							<td>
								<template v-if="resultsRendering == 'renderResults'">
									<span>Bg color (Row)</span>
									<br />
									<span>Fg color (Column)</span>
								</template>
								<template v-else>
									<br />
								</template>
							</td>
							<th v-for="(color) in colors" :key="color.id">{{ color.colorName }} <span v-if="displayOptions.showSample" :style="{ color: '#' + color.colorHex }">■</span> <span class="text-muted fw-normal" v-if="displayOptions.showHex"> <br> #{{ color.colorHex }}</span></th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(colorCol) in colors" :key="colorCol.id">
							<th>{{ colorCol.colorName }} <span v-if="displayOptions.showSample" :style="{ color: '#' + colorCol.colorHex }">■</span>  <span class="text-muted fw-normal" v-if="displayOptions.showHex"> <br> #{{ colorCol.colorHex }}</span></th>
							<template v-for="(colorRow) in colors" :key="colorRow.id">
								<td v-if="resultsRendering == 'renderResults'" :style="{ color: '#' + colorCol.colorHex, background: '#' + colorRow.colorHex}">
									{{ exampleText }}
								</td>
								<template v-else>
									<td v-if="calculateContrast(colorCol.colorLum, colorRow.colorLum) == 3" :style="matrixStyles(3)">
										{{ formatContrastResult(calculateContrast(colorCol.colorLum, colorRow.colorLum)) }}
									</td>
									<td v-else-if="calculateContrast(colorCol.colorLum, colorRow.colorLum) == 2" :style="matrixStyles(2)">
										{{ formatContrastResult(calculateContrast(colorCol.colorLum, colorRow.colorLum)) }}
									</td>
									<td v-else-if="calculateContrast(colorCol.colorLum, colorRow.colorLum) == 1" :style="matrixStyles(1)">
										{{ formatContrastResult(calculateContrast(colorCol.colorLum, colorRow.colorLum)) }}
									</td>
									<td v-else-if="calculateContrast(colorCol.colorLum, colorRow.colorLum) == 0" :style="matrixStyles(0)">
										{{ formatContrastResult(calculateContrast(colorCol.colorLum, colorRow.colorLum)) }}
									</td>
									<td v-else :style="matrixStyles(-1)">
										{{ formatContrastResult(calculateContrast(colorCol.colorLum, colorRow.colorLum)) }}
									</td>
								</template>
							</template>
						</tr>
					</tbody>
				</table>
			</div>
			<div v-else>
				<p class="text-muted">Add at least 2 colors to start generating a contrast table.</p>
			</div>
		</div>
	</div>
	<!-- Levels of success -->
    <div class="row mb-3">
		<SuccessLevels />
	</div>
	<!-- Color save -->
	<div class="row mb-3">
		<ColorSave :data="colors" @load-data="loadData" />
	</div>
	<!-- Color export -->
	<div class="row mb-3">
		<ColorExport :data="colors" :resultsRendering="resultsRendering" />
	</div>
</template>

<script>
	import AddColor from "./components/AddColor.vue"
	import ColorRow from "./components/ColorRow.vue"
	import SuccessLevels from "./components/SuccessLevels.vue"
	import ColorSave from "./components/ColorSave.vue"
	import ColorExport from "./components/ColorExport.vue"

	import ColorData from "./assets/ColorData.json"

export default {
	name: 'ContrastMatrix',
	data () {
		return {
			displayOptions: {
				showSample: true,
				showLum: false,
				showHex: false,
			},
			wcagLevel: 'AA',
			resultsRendering: "colorizeResults",
			exampleText: "Lorem Ipsum dolor sit amet",
			colors: [],
		}
	},
	computed: {
		nextId() {
			return this.colors.length
		},
		
	},
	components: {
		AddColor,
		ColorRow,
		SuccessLevels,
		ColorSave,
		ColorExport,
	},
	methods: {
		addColor(newColorName, newColorHex) {
			this.colors.push({
				id: this.nextId,
				colorName: newColorName,
				colorHex: standardizeHex(newColorHex),
				colorRGB: hexToRGB(newColorHex),
				colorLum: calculateLum(hexToRGB(newColorHex).r,hexToRGB(newColorHex).g,hexToRGB(newColorHex).b),
			})
		},
		updateHex(hex,id) {
			console.log(hex, id)
			console.log(this.colors[id])
			this.colors[id].colorHex = standardizeHex(hex),
			this.colors[id].colorRGB = hexToRGB(hex),
			this.colors[id].colorLum = calculateLum(hexToRGB(hex).r,hexToRGB(hex).g,hexToRGB(hex).b)
		},
		updateName(name, id) {
			this.colors[id].colorName = name
		},
		removeRow(id) {
			this.colors.splice(id, 1)
		},
		calculateContrast(lum1, lum2) {
			var ratio = lum1 > lum2 
			? ((lum2 + 0.05) / (lum1 + 0.05))
			: ((lum1 + 0.05) / (lum2 + 0.05))
			if (ratio <= 1/7) {
				return 3
			} else if (ratio <= 1/4.5) {
				return 2
			} else if (ratio <= 1/3) {
				return 1
			} else if (ratio < 1) {
				return 0
			} else {
				return -1
			}
		},
		formatContrastResult(contrastResult) {
			if (contrastResult == 3) {
				return "7:1"
			} else if (contrastResult == 2) {
				return "4.5:1"
			} else if (contrastResult == 1) {
				if (this.wcagLevel == 'AAA') {
				return "Fail"
				} else {
				return "3:1"
				}
			} else if (contrastResult == 0) {
				return "Fail"
			} else {
				return " "
			}
		},
		matrixStyles(success) {
			if (this.resultsRendering == 'colorizeResults') {
				if (success == -1 ) {
					return { background: '' }
				} else {
					if (this.wcagLevel == 'AAA') {
						if (success >= 3) {
							return { background: '#0f03' }
						}	else if (success == 2) {
							return { background: '#ff03' }
						} else {
							return { background: '#f113' }
						}
					} else if (this.wcagLevel == 'AA') {
						if (success >= 2) {
							return { background: '#0f03' }
						}	else if (success == 1) {
							return { background: '#ff03' }
						} else {
							return { background: '#f113' }
						}
					} else {
						if (success >= 1) {
							return { background: '#0f03' }
						}	else if (success == 0) {
							return { background: '#ff03' }
						} else {
							return { background: '#f113' }
						}
					}
				}
			} else {
				return { background: '' }
			}
		},
		loadData(data) {
			this.colors = JSON.parse(data)
		},
		defaultData() {
			this.colors = ColorData
		}
	}

}

function standardizeHex(value) {
	if (value.startsWith('#')) {
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
.table-assist {
	overflow: hidden;
  }
  .table-assist td,
  .table-assist th {
	position: relative;
	padding: 10px;
	outline: 0;
  }
  
  body:not(.nohover) .table-assist tbody tr:hover {
	background-color: #f882;
  }
  
  .table-assist tbody td:hover::after,
  .table-assist tbody td:focus::after, 
  .table-assist thead th:hover::after,
  .table-assist thead th:focus::after { 
	content: '';  
	height: 10000px;
	left: 0;
	position: absolute;  
	top: -5000px;
	width: 100%;
	z-index: -1;
  }
  
  .table-assist td:hover::after,
  .table-assist th:hover::after {
	background-color: #88f2;
  }
</style>
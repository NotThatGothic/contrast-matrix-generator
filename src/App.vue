<template>
		<div class="fixed-top container">
		<div class="accordion" id="accordionDisplayOptions">
			<div class="accordion-body">
				<div id="displayOptionsCollapse" class="accordion-collapse collapse bg-white border border-primary shadow-sm" aria-labelledby="displayOptionsHeading" data-bs-parent="#accordionDisplayOptions">
					<div class="accordion-body">
						<form class="form row" @submit.prevent>
							<div class="col-auto">
								<div class="form-check form-switch">
									<input class="form-check-input" type="checkbox" value="" id="readOnly" v-model="displayOptions.readOnly">
									<label class="form-check-label" for="readOnly">
										Read only mode
									</label>
								</div>
								<div class="form-check form-switch">
									<input class="form-check-input" type="checkbox" value="" id="palette" v-model="displayOptions.palette">
									<label class="form-check-label" for="palette">
										Palette mode
									</label>
								</div>
								<div class="form-check form-switch">
									<input class="form-check-input" type="checkbox" value="" id="resultsRendering" v-model="displayOptions.resultsRendering">
									<label class="form-check-label" for="resultsRendering">
										 Style table 
									</label>
								</div>
								<div class="form-check form-switch">
									<input class="form-check-input" type="checkbox" value="" id="showSample" v-model="displayOptions.showSample">
									<label class="form-check-label" for="showSample">
										Show sample
									</label>
								</div>
							</div>
							<div class="col-auto">
								<div class="form-check form-switch">
									<input class="form-check-input" type="checkbox" value="" id="showRGB" v-model="displayOptions.showRGB">
									<label class="form-check-label" for="showRGB">
										Show RGB value
									</label>
								</div>
								
								<div class="form-check form-switch">
									<input class="form-check-input" type="checkbox" value="" id="showHex" v-model="displayOptions.showHex">
									<label class="form-check-label" for="showHex">
										Show HEX value
									</label>
								</div>
								<div class="form-check form-switch">
									<input class="form-check-input" type="checkbox" value="" id="showLum" v-model="displayOptions.showLum">
									<label class="form-check-label" for="showLum">
										Show luminosity
									</label>
								</div>				
							</div>
							<div class="col-4">
								<div class="form-check form-switch">
									<input class="form-check-input" type="checkbox" value="" id="APCA" v-model="displayOptions.apca">
									<label class="form-check-label" for="APCA">
										APCA standards
									</label>
								</div>
								<div class="row">
									<div class="col">
										<fieldset :disabled="displayOptions.apca == true">
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
										</fieldset>
									</div>
									<div class="col">
										<fieldset :disabled="displayOptions.apca == true" class="form-check">
											<input class="form-check-input" type="checkbox" value="" id="showValues" v-model="displayOptions.showValues">
											<label class="form-check-label" for="showValues">
												Show Values
											</label>
										</fieldset>
										<fieldset :disabled="displayOptions.apca == true | (displayOptions.showValues == false)" >
											<div class="input-group">
												<span for="level-of-accuracy" class="input-group-text">Accuracy</span>
												<input id="level-of-accuracy" class="form-control" type="number" min="1" name="levelOfAccuracy" v-model="levelOfAccuracy" />
											</div>
										</fieldset>
									</div>
								</div>
							</div>
							<fieldset class="col">
								<label for="example-text" class="form-label">Example Text</label>
								<input id="example-text" class="form-control" type="text" name="example-text" v-model="exampleText" />
							</fieldset>
						</form>
					</div>
				</div>
			</div>
			<div class="position-absolute end-0 top-0 mt-3" style="margin-right: -2.5rem !important;">
				<button id="displayOptionsHeading" class="btn btn-light btn-outline-primary rounded-circle shadow-sm collapsed" style="height: 48px; width:48px " type="button" data-bs-toggle="collapse" data-bs-target="#displayOptionsCollapse" aria-expanded="false" aria-controls="displayOptionsCollapse">🛠️</button>
			</div>
		</div>
	</div>
	<!-- Add color -->
    <div class="row mb-5">
		<AddColor @add-color="addColor" />
	</div>
	<!-- Current colors -->
    <div class="row mb-5">
        <div class="col-12">
			<div class="row justify-content-between">
				<div class="col-auto">
					<h2>Current colors</h2>
				</div>
				<div class="col-auto text-end">
					<button class="btn btn-light btn-outline-info mb-1" @click="copyTable('current-colors-table')" :disabled="!colors.length" title="Copy table" >📋</button>
				</div>
			</div>
			<div v-if="colors.length == 0">
				<p class="text-muted">Add some colors with the input above, import colors from JSON below or <a role="button" class="text-link" @click="loadData(bootstrapColors)">load some example colors from Bootstrap</a>.</p>
			</div>
			<div style="overflow-x: auto; height:100%" v-else :key="renderKey">
				<div v-if="displayOptions.palette" id="current-colors-table" class="d-flex flex-wrap">
					<div v-for="(color) in colors" :key="color.id" is="vue:ColorRow" :id="color.id" :name="color.colorName" :hex="color.colorHex" :displayOptions="displayOptions" @update-name="updateName" @update-hex="updateHex" @remove-row="removeRow" />
				</div>
				<table v-else id="current-colors-table" class="table table-hover border">
					<thead>
						<tr>
							<th style="width:auto">Color name</th>
							<th style="width:20ex" class="text-center" v-if="displayOptions.showSample">Color sample</th>
							<th style="width:auto">Color hexcode</th>
							<th v-if="displayOptions.showRGB" style="width:auto">Color RGB value</th>
							<th style="width:auto" v-if="displayOptions.showLum">Color Lum</th>
							<th v-if="!displayOptions.readOnly"></th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(color) in colors" :key="color.id" is="vue:ColorRow" :id="color.id" :name="color.colorName" :hex="color.colorHex" :displayOptions="displayOptions" @update-name="updateName" @update-hex="updateHex" @remove-row="removeRow" />
					</tbody>
					<tfoot v-if="!displayOptions.readOnly">
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
        </div>
    </div>
	<!-- Contrast table -->
    <div class="row mb-5">
        <div class="col-12">
			<div class="row justify-content-between">
				<div class="col-auto">
					<h2 class="">Contrast table</h2>
				</div>
				<div class="col-auto text-end">
					<button class="btn btn-light btn-outline-info mb-1" @click="copyTable('contrast-matrix-table')" :disabled="colors.length < 2" title="Copy table" >📋</button>
				</div>
			</div>
			<div style="overflow-x:auto;" v-if="colors.length >= 2">
				<table id="contrast-matrix-table" ref="contrast" class="table table-bordered table-assist">
					<thead>
						<tr>
							<td>
								<br />
							</td>
							<th style="text-align: left;" v-for="(color) in colors" :key="color.id"><div style="display:inline-flex; justify-content:space-between; width:100%">{{ color.colorName }} <span v-if="displayOptions.showSample" style="padding-left:3px" :style="{ color: '#' + color.colorHex }">■</span></div><span class="text-muted" style="font-weight:normal" v-if="displayOptions.showHex"><br>#{{ color.colorHex }}</span></th>
						</tr>
					</thead>
					<tbody>
						<tr v-for="(colorCol) in colors" :key="colorCol.id">
							<th style="text-align: left;"><div style="display:inline-flex; justify-content:space-between; width:100%">{{ colorCol.colorName }} <span v-if="displayOptions.showSample" style="padding-left:3px" :style="{ color: '#' + colorCol.colorHex }">■</span></div> <span class="text-muted" style="font-weight:normal" v-if="displayOptions.showHex"> <br> #{{ colorCol.colorHex }}</span></th>
							<template v-for="(colorRow) in colors" :key="colorRow.id">
									<template v-if="displayOptions.apca == false">
										<td :style="matrixStyles(calculateContrast(colorCol.colorLum, colorRow.colorLum))">
											{{ calculateContrast(colorCol.colorLum, colorRow.colorLum) }}
										</td>
									</template>
									<template v-else>
										<td :style="apcaStyles(apcaContrast(colorCol.colorHex,colorRow.colorHex))">
											{{ apcaContrast(colorCol.colorHex,colorRow.colorHex) }}
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
    <div class="row mb-5">
		<SuccessLevels :displayOptions="displayOptions" />
	</div>
	<!-- Rendered sample -->
    <div class="row mb-5">
        <div class="col-12">
			<div class="row justify-content-between">
				<div class="col-auto">
					<h2 class="">Rendered sample</h2>
				</div>
				<div class="col-auto text-end">
					<button class="btn btn-light btn-outline-warning mb-1 me-2" type="button" data-bs-toggle="collapse" data-bs-target="#rendered-sample-content" aria-expanded="false" aria-controls="rendered-sample-content" :disabled="colors.length < 2" title="Toggle collapse rendered sample">👁️</button>
					<button class="btn btn-light btn-outline-info mb-1" @click="copyTable('rendered-sample-table')" :disabled="colors.length < 2" title="Copy table" >📋</button>
				</div>
			</div>
			<div class="collapse show" id="rendered-sample-content" style="overflow-x:auto;" v-if="colors.length >= 2">
				<table id="rendered-sample-table" class="table table-bordered">
					<tbody>
						<template v-for="(colorCol) in colors" :key="colorCol.id">
							<tr>
								<th> 
									{{ colorCol.colorName }} 
									<span v-if="displayOptions.showSample" style="padding-left:3px" :style="{ color: '#' + colorCol.colorHex }">■</span> </th>
							</tr>
							<tr v-for="(colorRow) in colors" :key="colorRow.id" :style="{ color: '#' + colorCol.colorHex, background: '#' + colorRow.colorHex}">
								<template v-if="colorCol.colorLum !== colorRow.colorLum">
									<td>
										<template v-if="displayOptions.apca == false">
												{{ calculateContrast(colorCol.colorLum, colorRow.colorLum) }}
										</template>
										<template v-else>
												{{ apcaContrast(colorCol.colorHex,colorRow.colorHex) }}
										</template>
									</td>
									<td>
										{{ exampleText }}
									</td>
								</template>
							</tr>
						</template>
					</tbody>
				</table>
			</div>
			<div v-else>
				<p class="text-muted">Add at least 2 colors to start rendering samples.</p>
			</div>
		</div>
	</div>
	<!-- Color export -->
	<div class="row">
		<ColorExport :data="colors" @load-data="loadData"/>
	</div>
</template>

<script>
	import AddColor from "./components/AddColor.vue"
	import ColorRow from "./components/ColorRow.vue"
	import SuccessLevels from "./components/SuccessLevels.vue"
	import ColorExport from "./components/ColorExport.vue"
	import ColorData from "./assets/ColorData.json"

	import { APCAcontrast, reverseAPCA, sRGBtoY, displayP3toY, adobeRGBtoY, alphaBlend, calcAPCA, fontLookupAPCA } from 'apca-w3';
	import { colorParsley, colorToHex, colorToRGB } from 'colorparsley';  // optional string parsing
export default {
	name: 'ContrastMatrix',
	mounted() {
		if(window.location.search) {
			this.loadData(getAllUrlParams(this.url))
		}
	},
	data () {
		return {
			displayOptions: {
				showSample: true,
				showLum: false,
				showHex: false,
				showRGB: true,
				showValues: false,
				palette: false,
				readOnly: false,
				apca: false,
				resultsRendering: true,
			},
			wcagLevel: 'AA',
			exampleText: "Lorem Ipsum dolor sit amet",
			levelOfAccuracy: 3,
			colors: [],
			bootstrapColors: ColorData,
			url: window.location.search,
			renderKey: 0
		}
	},
	watch: {
		compUrl(newUrl, oldUrl) {
			window.history.replaceState({}, '', '?'+newUrl );
		}
	},
	computed: {
		nextId() {
			if (this.colors.length) {
				return (this.colors[(this.colors.length - 1)].id + 1)
			} else {
				return 0
			}
		},
		compUrl() {
			if(this.colors){
				var urlParams = new URLSearchParams();
				this.colors.forEach((obj,i) => {
					urlParams.append([obj.colorName], [obj.colorHex]);
				})
			}
			return urlParams
		}
	},
	components: {
		AddColor,
		ColorRow,
		SuccessLevels,
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
			this.renderKey++;
		},
		updateHex(hex,id) {
			this.colors[id].colorHex = standardizeHex(hex),
			this.colors[id].colorRGB = hexToRGB(hex),
			this.colors[id].colorLum = calculateLum(hexToRGB(hex).r,hexToRGB(hex).g,hexToRGB(hex).b)
		},
		updateName(name, id) {
			this.colors[id].colorName = name
		},
		removeRow(id) {
			this.colors.splice(this.colors.findIndex(item => item.id == id), 1)
		},
		calculateContrast(lum1, lum2) {
			var ratio = lum1 > lum2 
			? ((lum2 + 0.05) / (lum1 + 0.05))
			: ((lum1 + 0.05) / (lum2 + 0.05))
			if (this.displayOptions.showValues == true) {
				return Math.floor((1 / ratio) * (10**this.levelOfAccuracy)) / (10**this.levelOfAccuracy) + ':1'
			} else {
				if (ratio <= 1/7) {
					return "7:1"
				} else if (ratio <= 1/4.5) {
					return "4.5:1"
				} else if (ratio <= 1/3) {
					if (this.wcagLevel == 'AAA') {
					return "Fail"
					} else {
					return "3:1"
					}
				} else if (ratio < 1) {
					return "Fail"
				} else {
					return " "
				}
			}
		},
		matrixStyles(ratio) {
			let num = ratio.split(":",1)
			if (this.displayOptions.resultsRendering == true) {
				if (num == " " || num == 1) {
					return { background: '' }
				} else {
					if (this.wcagLevel == 'AAA') {
						if (num >= 7) {
							return { background: '#0f03', fontWeight: 'bold' }
						}	else if (num >= 4.5) {
							return { background: '#ff03', fontWeight: 'bold' }
						}	else {
							return { background: '#f113' }
						}
					} else if (this.wcagLevel == 'AA') {
						if (num >= 4.5) {
							return { background: '#0f03', fontWeight: 'bold' }
						}	else if (num >= 3) {
							return { background: '#ff03', fontWeight: 'bold' }
						}	else {
							return { background: '#f113' }
						}
					} else {
						if (num >= 3) {
							return { background: '#0f03', fontWeight: 'bold' }
						}   else {
							return { background: '#f113' }
						}
					}
				}
			} else {
				return { background: '' }
			}	
		},
		apcaStyles(score) {
			let style = Math.abs(score);
			if (this.displayOptions.resultsRendering == true) {
					 if (style >= 90) {return { background: '#D4F5EB99', fontWeight: 'bold' }}
				else if (style >= 75) {return { background: '#91E4CC99', fontWeight: 'bold' }}
				else if (style >= 60) {return { background: '#F3AE5B99', fontWeight: 'bold' }}
				else if (style >= 45) {return { background: '#EB8F0099', fontWeight: 'bold' }}
				else if (style >= 30) {return { background: '#E3606C99', fontWeight: 'bold' }}
				else if (style >= 15) {return { background: '#D4334399', fontWeight: 'bold' }}
				else if (style >= 1)  {return { background: '#eeeeee99' }}
				else {return { background: '' }}
			}
		},
		loadData(data) {
			this.colors = []
			this.renderKey++;
			var isValidJSON = true;
			var regex = /%2B|%20|%25/gi
			try { JSON.parse(data) } catch {isValidJSON = false }
			if (isValidJSON) {data = JSON.parse(data)}
			for (const [key, value] of Object.entries(data)) {
				var parsed = key.replaceAll(regex, ' ')
				this.addColor(`${parsed}`, `${value}`) 
			}
		},
		copyTable(option) {
			let toCopy = document.querySelector('#'+option);
			selectNode(toCopy);
		},
		apcaContrast(text,bg) {
			// return Math.round(calcAPCA(text,bg));
			return Math.round((calcAPCA(text,bg) + Number.EPSILON) * 100) / 100;

		}
	}
}

function selectNode(node){
	let range  =  document.createRange();
	range.selectNodeContents(node)
	let select =  window.getSelection()
	select.removeAllRanges()
	select.addRange(range)  
	document.execCommand('copy')
	select.removeAllRanges()
}

function standardizeHex(value) {
	if (value.startsWith('#')) {
		value = value.substring(1, 7);
	}
	if (value.length == 3) {
		value = value[0] + value[0] + value[1] + value[1] + value[2] + value[2];
	}
	return value.toLowerCase();
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
function getAllUrlParams(url) {
	var queryString = url.split('?')[1];
	var obj = {};
	if (queryString) {
		queryString = queryString.split('#')[0];
		var arr = queryString.split('&');
		for (var i = 0; i < arr.length; i++) {
			var a = arr[i].split('=');
			var paramName = a[0];
			var paramValue = a[1];
			obj[paramName] = paramValue;
		}
	}
	return obj;
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
  
  .table-assist tbody td:hover::after,
  .table-assist tbody th:hover::after,
  .table-assist thead th:hover::after{
	background-color: #88f2;
  }
  .table-assist tbody td:hover {
	outline: solid 2px #f8f4 !important;
  }
  fieldset:disabled label {
    opacity: 0.5;
  }
</style>
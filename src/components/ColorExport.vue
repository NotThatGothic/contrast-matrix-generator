<template>
	<!-- Color export -->
	<div class="col-12">
		<h2>Matrix data</h2>
		<form class="form" id="export-table-form" @submit.prevent spellcheck="false">
			<div class="row">
				<div class="col">
					<div class="row">
						<div class="col">
							<textarea id="export-table" class="form-control mb-3" ref="textarea" style="" type="text" name="export-table" v-model="matrixData"/>
						</div>
					</div>
					<div class="row d-flex justify-content-between">
						<div class="col-auto mb-3 mb-md-0">
							<button class="btn btn-primary me-2" type="btn" @click="saveColorData()">Save to JSON</button>
							<button class="btn btn-primary" type="btn" @click="loadColorData()">Load JSON data</button>
						</div>
						<div class="col-auto mb-3 mb-md-0">
							<button class="btn btn-primary me-2" type="btn" @click="generateExportTable()">Contrast Table to HTML</button>
							<button class="btn btn-primary" type="btn" @click="copyUrl()">Share URL</button>
						</div>
					</div>
				</div>
				<div class="col-auto">
					<div class="d-flex flex-column">
						<button class="btn btn-light btn-outline-info mb-2" @click="copyExport" title="Copy export" >📋</button>
						<button class="btn btn-light btn-outline-danger" @click="this.matrixData=''" title="Clear export" >🗑️</button>
					</div>
				</div>
			</div>
		</form>
	</div>
</template>

<script>
	export default {
	name: 'ColorExport',
	props: {
		data: Array,
	},
	data() {
		return {
			matrixData: '',
			removedExport: '',
			colorDataString: ''
		}
	},
	methods: {
		generateExportTable() {
			let regexp = / data-v-[a-zA-Z0-9_]+=""| class=[a-zA-Z0-9_ \-""]+=""| [a-z-]+[a-zA-Z0-9_]+=""/g 
			this.removedExport = JSON.stringify(document.getElementById('contrast-matrix-table').outerHTML);
			this.removedExport = this.removedExport.replaceAll(/\\/g, '')
			this.removedExport = this.removedExport.replace(regexp, '')
			this.removedExport = this.removedExport.slice(0,-1)
			this.removedExport = this.removedExport.slice(1)
			this.matrixData = this.removedExport
		},
		saveColorData() {
			var urlParams = new URLSearchParams();
			if(this.data){
				var newObj = {}
				this.data.forEach((obj,i) => {
					Object.assign(newObj, {[obj.colorName]:obj.colorHex})
					urlParams.append([obj.colorName], [obj.colorHex]);
				})
				this.colorDataString = JSON.stringify(newObj)
				window.history.replaceState({}, '', '?'+urlParams );
			} else {
				this.colorDataString = ''
				window.history.replaceState({}, '',window.location.pathname) 
			}
			this.matrixData = this.colorDataString
		},
		loadColorData() {
			if (this.matrixData) {
				if (validate(this.matrixData)) {
					this.$emit('loadData', this.matrixData)
					this.matrixData = ''
				} else {
					alert('Invalid JSON')
				}
			} else {
				alert('Enter some JSON data to load')
			}
		},
		copyExport() {
			navigator.clipboard.writeText(this.$refs.textarea.value)
		},
		copyUrl() {
			navigator.clipboard.writeText(window.location)
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
	textarea {
		font-family: monospace;
		height: 10em;
	}
</style>
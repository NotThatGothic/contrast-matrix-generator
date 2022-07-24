<template>
	<!-- Color export -->
	<div class="col-12">
		<h2>Export table to HTML</h2>
		<form class="form row d-flex align-items-end" id="export-table-form" @submit.prevent spellcheck="false">
			<div class="col">
				<textarea id="export-table" class="form-control" style="" type="text" name="export-table" v-model="removedExport"/>
			</div>
			<div class="col-auto">
				<div class="d-flex flex-column">
					<button class="btn btn-primary" type="btn" v-on:click="generateExportTable()">Generate Export Table</button>
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
			colorExport: '',
			removedExport: ''
		}
	},
	methods: {
		generateExportTable() {
			let regexp = / data-v-[a-zA-Z0-9_]+=""| class=[a-zA-Z0-9_ \-""]+=""| [a-z-]+[a-zA-Z0-9_]+=""/g 
			this.colorExport = JSON.stringify(document.getElementById('contrast-matrix-table').outerHTML);
			this.removedExport = this.colorExport.replaceAll(/\\/g, '')
			this.removedExport = this.removedExport.replace(regexp, '')
			this.removedExport = this.removedExport.slice(0,-1)
			this.removedExport = this.removedExport.slice(1)
		}
	}
}

</script>

<style scoped lang="stylus">
	textarea {font-family: monospace; height: 10em}
</style>
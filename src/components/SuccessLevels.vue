<template>
	<!-- Levels of success -->
        <div class="col-12">
			<h3>Levels of success</h3>
			<p>Measured against the <a href="https://www.w3.org/TR/WCAG21/#contrast-minimum">WCAG 2.1 Success Criterion 1.4.3 Contrast guidelines</a></p>
			<ul>
				<li><strong>Fail</strong> - less than 3:1 level of contrast, use only for incidental, inactive, decorative elements.</li>
				<li><strong>3:1</strong> - at least 3:1 level of contrast, suitable for <a href="https://www.w3.org/TR/WCAG21/#dfn-large-scale">large-scale</a> text and images of text at AA standard.</li>
				<li><strong>4.5:1</strong> - at least 4.5:1 level of contrast, suitable for visual presentation of text and images of text at AA standard, or suitable for <a href="https://www.w3.org/TR/WCAG21/#dfn-large-scale">large-scale</a> text and images of text at AAA standard.</li>
				<li><strong>7:1</strong> - at least 7:1 level of contrast, suitable for visual presentation of text and images of text at AAA standard.</li>
			</ul>
        </div>
</template>

<script>
	export default {
	name: 'SuccessLevels'
}
</script>
<template>
	<!-- Levels of success -->
	<div class="col-12">
		<h3>Levels of success</h3>
		<template v-if="!displayOptions.apca">
			<p>Measured against the <a href="https://www.w3.org/TR/WCAG21/#contrast-minimum">WCAG 2.1 Success Criterion
					1.4.3 Contrast guidelines</a></p>
			<ul>
				<li><strong>Fail</strong> - less than 3:1 level of contrast, use only for incidental, inactive,
					decorative elements.</li>
				<li><strong>3:1</strong> - at least 3:1 level of contrast, suitable for <a
						href="https://www.w3.org/TR/WCAG21/#dfn-large-scale">large-scale</a> text and images of text at
					AA standard.</li>
				<li><strong>4.5:1</strong> - at least 4.5:1 level of contrast, suitable for visual presentation of text
					and images of text at AA standard, or suitable for <a
						href="https://www.w3.org/TR/WCAG21/#dfn-large-scale">large-scale</a> text and images of text at
					AAA standard.</li>
				<li><strong>7:1</strong> - at least 7:1 level of contrast, suitable for visual presentation of text and
					images of text at AAA standard.</li>
			</ul>
		</template>
		<template v-else>
			<figure>
				<p>
					ACPA uses <strong>Lightness contrast (L<sup>c</sup>)</strong> levels to show perceived readability contrast.
					<a href="https://git.apcacontrast.com/documentation/WhyAPCA">Read more about the new ACPA Standard
						here!</a>
						</p>
				<p>
					Scoring is more varied with the current proposed WCAG3 candidate, and varies based on use case. Below
					are the general level descriptions for L<sup>c</sup> values.</p>
				<ul>
					<li><strong>L<sup>c</sup>&nbsp;90</strong> <br>
						Preferred level for fluent text and columns of body text
						with a font no smaller than 18px/weight 300 or 14px/weight 400 (normal), or non-body text with a
						font no smaller than 12px/400. Also a recommended minimum for extremely thin fonts with a minimum of
						24px at weight 200. Lc&nbsp;90 is a <em>suggested maximum</em> for <strong>very large and bold
							fonts</strong> (greater than 36px bold), and large areas of color. Small fonts do not have a
						maximum.</li>
					<li><strong>L<sup>c</sup>&nbsp;75</strong> <br>
						The <em>minimum</em> level for columns of body text with a
						font no smaller than 24px/300 weight, 18px/400, 16px/500 and 14px/700. This level may be used with
						non-body text with a font no smaller than 15px/400. Also, L<sup>c</sup>&nbsp;75 should be considered
						a minimum for larger for any larger text where readability is important.</li>
					<li><strong>L<sup>c</sup>&nbsp;60</strong> <br>
						 The <em>minimum</em> level recommended for content text
						that is not body, column, or block text. In other words, text you want people to read. The minimums:
						no smaller than 48px/200, 36px/300, 24px normal weight (400), 21px/500, 18px/600, 16px/700 (bold).
						These values based on the reference font Helvetica. To use these sizes as body text, add
						L<sup>c</sup>&nbsp;15.</li>
					<li><strong>L<sup>c</sup>&nbsp;45</strong> <br>
						The <em>minimum</em> for larger, heavier text (36px normal
						weight or 24px bold) such as headlines, and large text that should be fluently readable but is not
						body text. This is also the minimum for pictograms with fine details, or smaller outline icons.</li>
					<li><strong>L<sup>c</sup>&nbsp;30</strong> <br>
						The <em>absolute minimum</em> for any text not listed
						above, including text considered as “spot readable”. This includes placeholder text and disabled
						element text, and some non-content like a copyright bug. This is also the minimum for large/solid
						semantic &amp; understandable non-text elements such as “mostly solid” icons or pictograms.
						Generally no less than 4px solid in its smallest dimension.</li>
					<li><strong>L<sup>c</sup>&nbsp;15</strong> <br>
						The <em>absolute minimum</em> for any non-text that needs
						to be <em>discernible</em>, and is no less than 6px (solid) in its smallest dimension. This may
						include dividers, and in <em>some</em> cases large buttons or thick focus visible outlines, but does
						<em>not</em> include fine details which have a higher minimum. <strong>Designers should treat
							anything below this level as invisible</strong>, as it will not be visible for many users. This
						minimum level should be avoided for any items important to the use, understanding, or interaction of
						the site.</li>
				</ul>
				<figcaption>
					Descriptions taken from <cite title="APCA in a Nutshell"><a href="https://git.apcacontrast.com/documentation/APCA_in_a_Nutshell.html#use-case-ranges">APCA in a Nutshell</a></cite>
				</figcaption>
			</figure>
		</template>
	</div>
</template>

<script>
	export default {
		name: 'SuccessLevels',
		props: {
			displayOptions: Object
		}
	}
</script>
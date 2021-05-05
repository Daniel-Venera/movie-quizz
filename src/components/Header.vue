<template>
	<div>
		<b-nav tabs>
			<b-nav-item disabled><b>Movie Quizz</b></b-nav-item>
			<b-nav-item disabled>{{numTotal !== 10 && numTotal !== 0 ? 'Result : ' + numCorrect + " / " + numTotal : ''}}</b-nav-item>
		</b-nav>
	</div>
</template>
<script>
export default {
	props: [
		'numCorrect',
		'numTotal'
	]
}
</script>

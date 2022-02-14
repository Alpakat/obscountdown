<script lang="ts">
	import style from '../countdownToTime.css';

	import { page } from '$app/stores';

	import moment from 'moment';

	let searchParams = $page.url.searchParams;
	let params = {
		color: searchParams.get('color') || 'black',
		size: searchParams.get('size') || '100',
		hour: searchParams.get('hour') || '23',
		minute: searchParams.get('minute') || '00',
		second: searchParams.get('second') || '00',
		onEnd: searchParams.get('onEnd') || 'stop'
	};

	let currentTime = new Date();

	setInterval(() => {
		currentTime = new Date();
	}, 1000);

	let timerString = '';

	let smallerText = false;

	$: {
		let timeToCountDown = new Date(
			currentTime.getFullYear(),
			currentTime.getMonth(),
			currentTime.getDate(),
			parseInt(params.hour),
			parseInt(params.minute),
			parseInt(params.second)
		);

		let timeRemaining = timeToCountDown.getTime() - currentTime.getTime();

		timerString = '';

		if (timeRemaining < 0) {
			if (params.onEnd == 'stop') {
				timeRemaining = 0;
			} else {
				timeRemaining = -timeRemaining;
				timerString = '-';
			}
		}

		if (timeRemaining < 60 * 60 * 1000) {
			timerString += moment(timeRemaining - 60 * 60 * 1000).format('mm:ss');
            smallerText = false
		} else {
			timerString += moment(timeRemaining - 60 * 60 * 1000).format('HH:mm:ss');
            smallerText = true
		}
	}
</script>

<div class="centerOnPage">
	<h1 style="font-size: {parseInt(params.size) - (smallerText ? 50 : 0)}px;">{timerString}</h1>
</div>

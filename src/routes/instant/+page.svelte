<script lang="ts">
	import { page } from '$app/stores';

	import dayjs from 'dayjs';

	const endIsoDate = $page.url.searchParams.get('end') || dayjs().toISOString();

	let endDate = dayjs(endIsoDate);
	let timer = {
		date: 0,
		hours: 0,
		mins: 0,
		seconds: 0
	};

	const getDuration = (start: dayjs.Dayjs, end: dayjs.Dayjs) => {
		const durations = dayjs.duration(end.diff(start));
		return {
			date: durations.days(),
			hours: durations.hours(),
			mins: durations.minutes(),
			seconds: durations.seconds()
		};
	};

	timer = getDuration(dayjs(), endDate);
	const countdownTimer = setInterval(() => {
		if (Object.entries(timer).every(([_, value]) => value === 0)) {
			clearInterval(countdownTimer);
			return;
		}
		timer = getDuration(dayjs(), endDate);
	}, 1000);
</script>

<div class="grid grid-flow-col gap-5 text-center auto-cols-max">
	<div class="flex flex-col">
		<span class="countdown font-mono text-5xl">
			<span style:--value={timer.date} />
		</span>
		days
	</div>
	<div class="flex flex-col">
		<span class="countdown font-mono text-5xl">
			<span style:--value={timer.hours} />
		</span>
		hours
	</div>
	<div class="flex flex-col">
		<span class="countdown font-mono text-5xl">
			<span style:--value={timer.mins} />
		</span>
		min
	</div>
	<div class="flex flex-col">
		<span class="countdown font-mono text-5xl">
			<span style:--value={timer.seconds} />
		</span>
		sec
	</div>
</div>

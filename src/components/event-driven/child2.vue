<template>
	<section class="child2">
		<h3>Child Component</h3>
		<h4>Child State: {{ state2 }}</h4>

		<Counter>
			<!-- input -->
			<template v-slot:input>
				<input v-model="incrementBy" id="n" type="number" step="1" min="1" />
			</template>

			<!-- tune -->
			<template v-slot:tune>
				<button
					class="btn btn-add"
					@mousedown="
						add(incrementBy);
						$emit('emited-counter2', [$event, state2]);
					"
				>
					+ {{ incrementBy }}
				</button>
				<button
					class="btn btn-substract"
					@mousedown="
						substract(incrementBy);
						$emit('emited-counter2', [$event, state2]);
					"
				>
					- {{ incrementBy }}
				</button>
			</template>
		</Counter>

		<!-- GrandChild -->
		<GrandChild2
			:state="state2"
			@emited-counter3="
				counter(...$event);
				$emit('emited-counter2', [$event, state2]);
			"
		></GrandChild2>
	</section>
</template>

<script lang="ts">
	import { ref, watchEffect } from "vue";

	import Counter from "../counter.vue";
	import GrandChild2 from "./grand-child2.vue";

	export default {
		name: "Child2", // devtools
		components: { Counter, GrandChild2 },
		props: { state: Number },
		// Events
		emits: {
			["emited-counter2"](e: Event, payload: number) {
				return [e, payload];
			},
		},
		setup(props: any) {
			const limitN = 1;

			// attr
			let incrementBy = ref(limitN);
			let state2 = ref(props.state);

			// methods
			function counter(e: Event, payload: number) {
				state2.value = payload;
			}

			function add(n: number): void {
				state2.value += +n;
			}

			function substract(n: number): void {
				state2.value -= +n;
			}

			// watch
			watchEffect(() => (state2.value = props.state));

			return { incrementBy, state2, counter, add, substract };
		},
	};
</script>

<style lang="scss" scoped>
	.child2 {
		padding: 1rem;
		background-color: palegoldenrod;

		h4 {
			color: blueviolet;
			padding: 1rem;
			background-color: gainsboro;
			border: 1px solid gray;
			border-radius: 5px;
			max-width: 180px;
			margin: 0 auto;
		}
	}
</style>

<script>
export default {
	data() {
		return {
			themeSelected: 0,
			calculRaw: "",
			screenDraw: "",
			calculator: [
				{
					name: "7",
				},
				{
					name: "8",
				},
				{
					name: "9",
				},
				{
					name: "DEL",
				},
				{
					name: "4",
				},
				{
					name: "5",
				},
				{
					name: "6",
				},
				{
					name: "+",
				},
				{
					name: "1",
				},
				{
					name: "2",
				},
				{
					name: "3",
				},
				{
					name: "-",
				},
				{
					name: ".",
				},
				{
					name: "0",
				},
				{
					name: "/",
				},
				{
					name: "x",
				},
			],
		};
	},
	methods: {
		newNumber(data) {
			var ctx = this;

			if (data == "x") data = "*";

			if (!isNaN(data) || data == ".") {
				if (ctx.calculRaw == "") ctx.screenDraw = "";
				if (ctx.screenDraw.includes(".") && data == ".") {
					return;
				}
				ctx.calculRaw = ctx.calculRaw + data;
				ctx.screenDraw = ctx.screenDraw + data;
			} else {
				if (data == "DEL") {
					ctx.screenDraw = "";
					ctx.calculRaw = "";
					return;
				} else if (data == "RESET") {
					ctx.screenDraw = "";
					ctx.calculRaw = "";
					return;
				}

				let last = ctx.calculRaw[ctx.calculRaw.length - 1];
				if (isNaN(last)) {
					return;
				} else {
					if (data == "RESULT") {
						try {
							ctx.screenDraw = eval(ctx.calculRaw);
							ctx.calculRaw = "";
						} catch {
							ctx.screenDraw = "";
							ctx.calculRaw = "";
						}

						return;
					} else {
						ctx.screenDraw = "";
						ctx.calculRaw = ctx.calculRaw + data;
						return;
					}
				}
			}
		},
		changeTheme(id) {
			var ctx = this;
			ctx.themeSelected = id;

			let body = document.getElementsByTagName("body")[0];

			if (ctx.themeSelected == 0) body.classList.value = "";
			else body.classList.value = "theme-" + id;
		},
	},
};
</script>

<template>
	<main>
		<section class="calculator">
			<div class="top">
				<h1 class="name">calc</h1>
				<div class="theme-selector">
					<div class="left">THEME</div>
					<div class="right">
						<div class="number-list">
							<div class="number">1</div>
							<div class="number">2</div>
							<div class="number">3</div>
						</div>
						<div class="bar">
							<div
								:class="{
									level: 1,
									'hover-1': 1,
									'theme-1': themeSelected == 0,
								}"
								v-on:click="changeTheme(0)"
							></div>
							<div
								class="level"
								:class="{
									level: 1,
									'hover-2': 1,
									'theme-2': themeSelected == 1,
								}"
								v-on:click="changeTheme(1)"
							></div>
							<div
								class="level"
								:class="{
									level: 1,
									'hover-3': 1,
									'theme-3': themeSelected == 2,
								}"
								v-on:click="changeTheme(2)"
							></div>
						</div>
					</div>
				</div>
			</div>
			<div class="screen">
				<div class="number">
					{{ screenDraw == 0 ? 0 : screenDraw }}
				</div>
			</div>
			<div class="keyboard">
				<div class="grid">
					<div
						class="button"
						v-for="(value, index) in calculator"
						:key="index"
						v-on:click="newNumber(value.name)"
						:class="{
							del: value.name == 'DEL',
							'normal-btn': value.name != 'DEL',
						}"
					>
						{{ value.name }}
					</div>
				</div>
				<div class="bottom">
					<div class="big-btn reset" v-on:click="newNumber('RESET')">
						RESET
					</div>
					<div
						class="big-btn result"
						v-on:click="newNumber('RESULT')"
					>
						=
					</div>
				</div>
			</div>
		</section>
	</main>
</template>
<script setup lang="ts">
import { defineCustomElements } from '@ionic/pwa-elements/loader'
const { data, refresh } = useLazyFetch<{
	oficial: {
		price: number
	}
	blue: {
		ask: number
		bid: number
	}
	cripto: {
		usdt: {
			ask: number
			bid: number
		}
	}
}>('https://criptoya.com/api/dolar')

const toARS = ref(true)
const input = ref(0)

async function handleRefresh(event: unknown) {
	await refresh()
	await new Promise(r => setTimeout(r, 2000))
	// @ts-ignore
	event.target.complete()
}

function convert(value: number) {
	if (!input.value) return value
	if (toARS.value) {
		return value * input.value
	} else {
		return input.value / value
	}
}

onMounted(() => {
	defineCustomElements(window)
})
</script>

<template>
	<ion-page>
		<ion-header>
			<ion-toolbar color="primary">
				<ion-title>Simple Dólar</ion-title>
				<!-- <ion-buttons slot="end">
					<ion-button>
						<ion-icon
							slot="icon-only"
							:icon="ioniconsInformationCircle"
						/>
					</ion-button>
				</ion-buttons> -->
			</ion-toolbar>
		</ion-header>
		<ion-content v-if="data" class="ion-padding">
			<ion-refresher slot="fixed" @ionRefresh="handleRefresh($event)">
				<ion-refresher-content />
			</ion-refresher>
			<DisplayPrice
				class="border-b"
				title="Dólar Oficial"
				:price="convert(data.oficial.price)"
			/>
			<DisplayPrice
				class="border-b"
				title="Dólar Blue"
				:ask="convert(data.blue.ask)"
				:bid="convert(data.blue.bid)"
			/>
			<DisplayPrice
				title="Dólar Cripto"
				:ask="convert(data.cripto.usdt.ask)"
				:bid="convert(data.cripto.usdt.bid)"
			/>
		</ion-content>
		<div class="text-black bg-white p-4">
			<ion-input
				label="Convertir"
				label-placement="stacked"
				placeholder="0"
				v-model.number="input"
				type="number"
				fill="outline"
			>
				<ion-label slot="start">$</ion-label>
				<ion-button
					slot="end"
					shape="round"
					fill="outline"
					color="dark"
					size="small"
					class="text-xs"
					@click="toARS = !toARS"
				>
					{{ toARS ? 'USD' : 'ARS' }}
					<ion-icon
						:icon="ioniconsArrowForward"
						class="text-sm mx-2"
					/>
					{{ toARS ? 'ARS' : 'USD' }}
				</ion-button>
			</ion-input>
		</div>
	</ion-page>
</template>

<style scoped>
ion-button {
	--padding-start: 0.75rem;
	--padding-end: 0.75rem;
	--padding-top: 0.5rem;
	--padding-bottom: 0.5rem;
}
</style>

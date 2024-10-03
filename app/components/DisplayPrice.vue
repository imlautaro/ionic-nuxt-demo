<script setup lang="ts">
import { Toast } from '@capacitor/toast'
import { Haptics, ImpactStyle } from '@capacitor/haptics'

defineProps<{
	title: string
	price?: number
	bid?: number
	ask?: number
}>()

async function copyToClipboard(text: string | number) {
	navigator.clipboard.writeText(text.toString())
	await Haptics.impact({ style: ImpactStyle.Light })
	await Toast.show({ text: '¡Copiado!' })
}
</script>

<template>
	<div class="flex flex-col p-4 gap-2">
		<span class="text-md">{{ title }}</span>
		<span
			v-if="price"
			class="text-2xl whitespace-nowrap overflow-ellipsis overflow-hidden"
		>
			$<ion-label
				button
				@click="
					copyToClipboard(
						price.toLocaleString('es-AR', {
							currency: 'ARS',
							maximumFractionDigits: 2,
						})
					)
				"
			>
				{{
					price.toLocaleString('es-AR', {
						currency: 'ARS',
						maximumFractionDigits: 2,
					})
				}}
			</ion-label>
		</span>
		<div v-else-if="ask && bid" class="grid grid-cols-2 gap-4">
			<div class="flex flex-col overflow-hidden">
				<span class="text-sm text-gray-600">Compra</span>
				<span
					class="text-2xl whitespace-nowrap overflow-ellipsis overflow-hidden"
					>$<ion-label
						button
						@click="
							copyToClipboard(
								ask.toLocaleString('es-AR', {
									currency: 'ARS',
									maximumFractionDigits: 2,
								})
							)
						"
					>
						{{
							ask.toLocaleString('es-AR', {
								currency: 'ARS',
								maximumFractionDigits: 2,
							})
						}}
					</ion-label>
				</span>
			</div>
			<div class="flex flex-col overflow-hidden">
				<span class="text-sm text-gray-600">Venta</span>
				<span
					class="text-2xl whitespace-nowrap overflow-ellipsis overflow-hidden"
					>$<ion-label
						button
						@click="
							copyToClipboard(
								bid.toLocaleString('es-AR', {
									currency: 'ARS',
									maximumFractionDigits: 2,
								})
							)
						"
					>
						{{
							bid.toLocaleString('es-AR', {
								currency: 'ARS',
								maximumFractionDigits: 2,
							})
						}}
					</ion-label>
				</span>
			</div>
		</div>
	</div>
</template>

<script setup>
const props = defineProps({
	open: { type: Boolean, required: true },
	hideModal: { type: Function, required: true },
	title: { type: String, required: true },
	backdropClose: { type: Boolean, default: true },
	bodyPadding: { type: Boolean, default: true },
});

const emit = defineEmits(['closed']);

const hideModalHandler = () => {
	props.hideModal();

	emit('closed');
};
</script>

<template>
	<Teleport to="#modal-container">
		<!-- Main modal -->
		<div
			tabindex="-1"
			class="overflow-y-auto overflow-x-hidden w-screen h-screen fixed top-0 right-0 left-0 z-50 md:inset-0 h-modal md:h-full flex justify-center bg-gray-900/50 items-center"
			v-if="open"
		>
			<!-- :class="{ hidden:  }" -->
			<div
				v-if="backdropClose"
				class="w-full h-full absolute top-0 left-0 -z-10"
				@click="hideModalHandler"
			/>
			<div
				class="relative px-2 md:p-4 w-full max-w-xl md:h-auto xl:max-w-3xl"
			>
				<!------ modal content ------>
				<section class="relative bg-white rounded-lg shadow">
					<!------ modal header ------>
					<header
						class="flex justify-between items-start px-4 pt-4 pb-2 rounded-t border-b"
					>
						<slot name="title">
							<h3 class="capitalize text-lg text-secondary-base">
								{{ title }}
							</h3>
						</slot>
						<button
							type="button"
							class="text-gray-400 bg-transparent hover:bg-gray-200 hover:text-gray-900 rounded-lg text-sm p-1.5 ms-auto inline-flex items-center"
							@click="hideModalHandler"
						>
							<div class="w-6 h-6">
								<svg
									fill="none"
									viewBox="0 0 24 24"
									stroke-width="1.5"
									stroke="currentColor"
									class="w-6 h-6"
								>
									<path
										stroke-linecap="round"
										stroke-linejoin="round"
										d="M6 18L18 6M6 6l12 12"
									/>
								</svg>
							</div>
						</button>
					</header>

					<!------ modal body ------>
					<div
						class="space-y-6 max-h-[70vh] overflow-auto"
						:class="{ 'p-6': bodyPadding }"
					>
						<slot />
					</div>

					<!------ modal footer ------>
					<footer
						v-if="$slots.footer"
						class="capitalize flex items-center p-4 space-x-2 rounded-b border-t border-solid border-gray-200"
					>
						<slot name="footer" />
					</footer>
				</section>
			</div>
		</div>
	</Teleport>
</template>

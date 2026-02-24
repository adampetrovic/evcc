<template>
	<YamlModal
		id="tariffsModal"
		:title="$t('config.tariffs.title')"
		:description="$t('config.tariffs.description')"
		docs="/docs/tariffs"
		:defaultYaml="defaultYaml"
		removeKey="tariffs"
		endpoint="/config/tariffs"
		data-testid="tariffs-modal"
		@changed="$emit('changed')"
	>
		<template #afterDescription>
			<div class="form-check form-switch my-3">
				<input
					id="solarCostIncluded"
					:checked="solarCostIncluded"
					class="form-check-input"
					type="checkbox"
					role="switch"
					@change="changeSolarCostIncluded"
				/>
				<div class="form-check-label">
					<label for="solarCostIncluded">
						{{ $t("config.tariffs.solarCostIncluded.label") }}
					</label>
					<div class="text-muted small">
						{{ $t("config.tariffs.solarCostIncluded.description") }}
					</div>
				</div>
			</div>
		</template>
	</YamlModal>
</template>

<script>
import YamlModal from "./YamlModal.vue";
import defaultYaml from "./defaultYaml/tariffs.yaml?raw";
import store from "@/store";
import api from "@/api";

export default {
	name: "TariffsModal",
	components: { YamlModal },
	emits: ["changed"],
	data() {
		return { defaultYaml: defaultYaml.trim() };
	},
	computed: {
		solarCostIncluded() {
			return store.state.solarCostIncluded !== false;
		},
	},
	methods: {
		async changeSolarCostIncluded(e) {
			const val = e.target.checked;
			try {
				await api.post(`solarcostincluded/${val}`);
			} catch (err) {
				console.error(err);
				e.target.checked = !val;
			}
		},
	},
};
</script>

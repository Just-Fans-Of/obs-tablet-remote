<template>
	<div class="top-bar">
		<div class="item" :title="obs.version ? 'OBS Websocket Version: ' + obs.version : false">
			Connection: <span :class="connectionStateClass" v-text="connectionStateText"></span>
		</div>
		<div class="item">
			<span :class="streamingStateClass" v-text="streamingStateText"></span>
		</div>
		<div class="space"></div>
		<div class="item">
			<button @click="toggleStreaming" :class="streamingButtonClass" v-text="streamingButtonText"></button>
		</div>
		<div class="item">
			<button @click="forceRefresh" title="Force Refresh (In case things get out of sync)"><i class="material-icons">refresh</i></button>
		</div>
		<div class="item">
			<button @click="toggleSettings"><i class="material-icons">settings</i></button>
		</div>
		<div class="item">
			<button @click="toggleFullscreen"><i class="material-icons">fullscreen</i></button>
		</div>
	</div>
</template>

<script>
	import toggleFullscreenMixin from '../mixins/fullscreen'
	import OBSUserMixin from '../mixins/obs-user'

	export default {
		mixins: [toggleFullscreenMixin, OBSUserMixin],

		computed: {
			connectionStateClass() {
				if (!this.obs.connected) {
					return 'error-text'
				}
				if (this.obs.authRequired) {
					return 'warning-text'
				}
				return 'success-text'
			},
			connectionStateText() {
				if (!this.obs.connected) {
					return 'Down'
				}
				if (this.obs.authRequired) {
					return 'Authenticate'
				}
				return 'Ok'
			},

			streamingStateClass() {
				if (!this.obs.connected) {
					return 'hidden'
				}
				if (!this.obs.streaming) {
					return 'stream-down'
				}

				return 'stream-up'
			},
			streamingStateText() {
				if (!this.obs.connected) {
					return 'Doop'
				}

				return this.obs.streaming ? 'On Air' : 'Off Air'
			},
			streamingButtonText() {
				if (!this.obs.connected) {
					return 'Doop'
				}

				return this.obs.streaming ? 'Stop Streaming' : 'Start Streaming'
			},
			streamingButtonClass() {
				if (!this.obs.connected) {
					return 'hidden'
				}

				return 'stream-up'
			}

		},
		methods: {
			forceRefresh() {
				this.$emit('force-refresh')
			},
			toggleSettings() {
				this.$emit('toggle-settings')
			},
			toggleStreaming() {
				this.$obs.startStopStreaming();
			}
		},
		props: {
			obs: {
				type: Object,
				required: true
			}
		}
	}
</script>

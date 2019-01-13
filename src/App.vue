<template>
	<main id="app">
		<section class="center">
			<div class="step">
				<div class="step-bullet" :class="{'done': oauthToken}">
					<span v-show="!oauthToken">1</span>
					<i v-show="oauthToken" class="fas fa-check"></i>
				</div>
				<a :href="oauthHref" target="_self" class="step-button" :class="[oauthToken ? 'green' : 'blue']">
					<p>{{oauthToken ? 'Token generated' :'Generate token'}}</p>
					<i class="fas right" :class="[oauthToken ? 'fa-check' : 'fa-arrow-right']"></i>
				</a>
			</div>

			<div class="step">
				<div class="step-bullet" >
					<span>2</span>
				</div>
				<div class="step-button" :class="[oauthToken ? 'blue' : '', showToken ? 'show' : '']" @mouseover="showToken = true" @mouseleave="showToken = false">
					<div v-if="oauthToken" class="warning">
						<p>Hover to show token</p>
						<p>Do not show to steam!</p>
					</div>
					<p v-if="oauthToken" class="token">{{oauthToken}}</p>
					<p v-if="!oauthToken" style="color: initial;" title="You need to generate a token first">Copy token</p>
					<i class="fas fa-copy right"></i>
				</div>
			</div>
			
			<div class="step">
				<div class="step-bullet warn">
					<svg width="15px" viewBox="0 0 14 17" xmlns="http://www.w3.org/2000/svg" fill-rule="evenodd" clip-rule="evenodd" stroke-linejoin="round" stroke-miterlimit="1.414"><path d="M13.5 9.28v.926a.545.545 0 0 1-.546.545h-2.695a.545.545 0 0 1-.546-.545V9.28c0-.264.188-.484.438-.534V7.703a.338.338 0 0 0-.338-.337h-.111a2.853 2.853 0 0 1-2.506 1.467 2.853 2.853 0 0 1-2.505-1.467H.546A.545.545 0 0 1 0 6.821v-1.8c0-.301.244-.545.546-.545h4.201A2.874 2.874 0 0 1 6.236 3.27v-.677c0-.038.003-.074.011-.109H4.781a.546.546 0 0 1-.546-.545v-.781c0-.301.245-.545.546-.545h1.454V.545c0-.301.245-.545.546-.545h.83c.302 0 .546.244.546.545v.068h1.454c.302 0 .546.244.546.545v.781a.545.545 0 0 1-.546.545H8.146a.552.552 0 0 1 .011.109v.677a2.874 2.874 0 0 1 1.489 1.206h1.05a2.331 2.331 0 0 1 2.331 2.327V8.74a.546.546 0 0 1 .473.54zm-.568 5.234a.518.518 0 0 0-.028-.095l-.812-2.581a.544.544 0 0 0-1.041 0l-.824 2.62-.011.039a1.422 1.422 0 1 0 2.777.424c0-.137-.021-.274-.061-.407z" fill="#fff" fill-rule="nonzero"/></svg>
				</div>
				<a href="https://www.twitch.tv/settings/connections" target="_blank" class="step-button orange" title="If you leaked your key or simply want to remove permissions, click here and remove it from twitch">
					<p>Delete token</p>
					<i class="fas fa-arrow-right right"></i>
				</a>
			</div>
		</section>
  	</main>
</template>

<script>
export default {
  	name: 'app',
  	data() {
    	return {
			  oauthToken: null,
			  showToken: false
    	}
  	},
  	created() {
		// Store token
		const token = document.location.hash.match(/\#(?:access_token)\=([\S\s]*?)\&/)
		
		if (token) {
			this.oauthToken = "oauth:" + document.location.hash.match(/\#(?:access_token)\=([\S\s]*?)\&/).pop();
			//Hide token from addressbar in case someone openes on stream
			document.location.hash = "token-success"
		}
  	},
  	computed: {
		oauthHref() {
			const clientId = "9058tygryan4mexx61jc39a30wl4zl"
			const redirectUri = "https://n0.no/twitch/"
			const scope = "chat:read+chat:edit+channel:moderate"
			return `https://id.twitch.tv/oauth2/authorize?client_id=${clientId}&redirect_uri=${redirectUri}&response_type=token&scope=${scope}`
		}
	  }
}
</script>

<style lang="scss">
body, html {
	height: 100%;
	background: #212121;
}
main {
  	font-family: 'Avenir', Helvetica, Arial, sans-serif;
  	-webkit-font-smoothing: antialiased;
  	-moz-osx-font-smoothing: grayscale;
  	text-align: center;
  	color: #2c3e50;
  	padding-top: 60px;
	.center {
		display: flex;
		justify-content: center;
		width: 24rem;
		margin: auto;
		flex-wrap: wrap;
		position: relative;

		&::before {
			content: '';
			height: 500px;
			width: 10px;
			background-color: #F7F9FD;
			top: 5px;
			position: absolute;
		}
	}

	.step {
		display: flex;
		justify-content: center;
		flex-wrap: wrap;
		z-index: 2;
		margin-top: 60px;

		&:first-child {
			margin-top: 0;
		}

		&-bullet {
			width: 30px;
			height: 30px;
			background-color: #87879B;
			color: #f7f9fd;
			border-radius: 50%;
			display: flex;
			justify-content: center;
			align-items: center;
			&::after {
				content: '';
				width: 45px;
				height: 45px;
				position: absolute;
				background-color: rgba(135, 135, 155, 0.7);
				z-index: -1;
				border-radius: 50%;
			}

			&.done {
				@extend .step-bullet;
				background-color: #1789F5;
				&::after {
					background-color: rgba(23, 137, 245, .7)
				}
			}
			&.warn {
				@extend .step-bullet;
				background-color: #f54a17;
				&::after {
					background-color: rgba(245, 75, 23, .7)
				}
			}
		}
		&-button {
			margin-top: 50px;
			background: #ddd;
			position: relative;
			width: 24rem;
			height: 3.5rem;
			font-size: 1.3rem;
			border-radius: 30px;
			display: flex;
			align-items: center;
			justify-content: center;
			color: #F7F9FD;
			text-decoration: none;

			&.green {
				background-color: #17CDBD;
			}
			&.blue {
				background-color: #1789F5;
			}
			&.orange {
				background-color: #f54a17;
			}

			.right {
				position: absolute;
    			right: 20px;
			}

			&.show {
				.warning {
					display: none;
				}
				.token {
					filter: blur(0);
					opacity: 1;
				}
			}

			.warning {
				position: absolute;
				margin: auto;
				p {
					margin: 0;
					&:first-of-type {
					font-size: .6em;
					}
				}
			}
			.token {
				font-size: 1rem;
				margin: 0;
				filter: blur(2px);
				opacity: 0.5;
				padding: 20px 30px;
				&::selection {
					background-color: #17CDBD;
				}
			}

		}
	}
}
</style>

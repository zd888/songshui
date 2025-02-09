<script>
	export default {
		onLaunch: function() {
			
		},
		onShow: function() {

		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	/*每个页面公共css */
	@import "uview-ui/index.scss";
	@import 'components/colorui/main.css';
	@import 'components/colorui/icon.css';

	page {
		background-color: #FFFFFF;
		color: #343546;
	}

	.bg {
		background-color: #F7F7F7;
	}
</style>
<script>
	export default {
		onLaunch: function() {
			console.log('App Launch')
		},
		onShow: function() {
			console.log('App Show')
		},
		onHide: function() {
			console.log('App Hide')
		}
	}
</script>

<style lang="scss">
	@import "uview-ui/index.scss";
	@import './common/common.css';
	
	body{
		background-image: linear-gradient(to right top, #79277e, #6d2571, #612265, #552059, #4a1d4d, #4a1d4d, #4a1d4d, #4a1d4d, #552059, #612265, #6d2571, #79277e);	
		}
</style>

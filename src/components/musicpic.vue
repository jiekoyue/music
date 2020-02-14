<template>
    <div class="player_con">
        <img src="../assets/images/player_bar.png" class="play_bar" :class="{playing:playing}"/>
        <!-- 黑胶碟片 -->
        <img src="../assets/images/disc.png" class="disc autoRotate" :class="{playing:playing}"/>
        <img :src="picurl" class="cover autoRotate" :class="{playing:playing}"/>
        <img src="../assets/images/cover.png" style="display: none" ref="hideimg"/>
    </div>
</template>

<script>
	export default {
		name: "musicpic",
		data() {
			return {
				picurl: '',
				playing:false
			};
		},
        methods:{

        },
		mounted() {
			this.picurl = this.$refs.hideimg.src;
		}
	}
</script>

<style scoped>

</style>
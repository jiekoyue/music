<template>
    <div class="song_wrapper" ref="wrapp">
        <ul class="song_list">
            <li v-for="(time,index) in listarr" :key="index" :class="{newli:time.id==liid}" @click="gqclick(time.id)">
                {{time.name}}--{{time.artists[0].name}}
            </li>
        </ul>
    </div>
</template>

<script>
	import iscroll from 'iscroll';

	export default {
		name: "musiclist",
		data() {
			return {
				listarr: '',
				liid: '',
				myis: ''
			};
		},
		methods: {
			gqclick(id) {
				this.liid = id;
				this.$parent.gqclick(id);
			},
			updata(data) {
				this.listarr = data;
				this.$nextTick(() => {
					this.myis.scrollTo(0, 0);
					this.myis.refresh();
				});

			}
		},
		mounted() {
			this.myis = new iscroll(this.$refs.wrapp, {
				mouseWheel: true,
				scrollbars: true,
				interactiveScrollbars: true
			})
		}
	}
</script>

<style scoped>

</style>
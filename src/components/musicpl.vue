<template>
    <div class="comment_list" ref="wrapp">
        <div>
            <dl v-for="(time,index) in plarr" :key="index">
                <dt>
                    <img :src="time.user.avatarUrl"
                         alt=""/>
                </dt>
                <dd class="name">{{time.user.nickname}}</dd>
                <dd class="detail">
                    {{time.content}}
                </dd>
            </dl>
        </div>
    </div>
</template>

<script>
	import iscroll from 'iscroll';

	export default {
		name: "musicpl",
		data() {
			return {
				plarr: '',
				myis: ''
			};
		},
		methods: {
			updata(data) {
				this.plarr = data;
				this.$nextTick(() => {
					this.myis.refresh();
					this.myis.scrollTo(0, 0);
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
<template>
    <div class="wrap">
        <div class="play_wrap" id="player">
            <div class="search_bar">
                <img src="./assets/images/player_title.png" alt=""/>
                <input type="text" v-model="inpVal" @keyup.13="search"/>
            </div>
            <div class="center_con">
                <!-- 歌曲列表-->
                <musiclist ref="gqlist"></musiclist>
                <!-- 唱片-->
                <musicpic ref="alpic"></musicpic>
                <!-- 评论列表-->
                <musicpl ref="pllist"></musicpl>
            </div>
            <div class="audio_con">
                <audio :src="musicurl" @pause="over" @play="bging" controls autoplay loop class="myaudio"></audio>
            </div>
        </div>
    </div>
</template>

<script>
	import musiclist from './components/musiclist.vue';
	import musicpic from './components/musicpic.vue';
	import musicpl from './components/musicpl.vue';
	import axios from 'axios';

	export default {
		name: 'App',
		components: {
			musiclist,
			musicpic,
			musicpl
		},
		data() {
			return {
				inpVal: '',
				musicurl: ''
			};
		},
		methods: {
			search() {
				axios({url: 'http://183.237.67.218:3000/search?keywords=' + this.inpVal}).then(msg => {
					window.console.log(msg);
					this.$refs.gqlist.updata(msg.data.result.songs);
				});
			},
			gqclick(id) {
				this.musicurl = '';
				axios({url: 'http://183.237.67.218:3000/song/url?id=' + id}).then(msg => {
					window.console.log(msg);
					this.musicurl = msg.data.data[0].url;
				});
				axios({url: 'http://183.237.67.218:3000/comment/music?id=' + id}).then(msg => {
					window.console.log(msg);
					this.$refs.pllist.updata(msg.data.hotComments.concat(msg.data.comments));
				});
				axios({url: 'http://183.237.67.218:3000/song/detail?ids=' + id}).then(msg => {
					window.console.log(msg);
					this.$refs.alpic.picurl = msg.data.songs[0].al.picUrl;
				});
			},
			bging() {
				this.$refs.alpic.playing = true;
			},
			over() {
				this.$refs.alpic.playing = false;
			}
		},
		mounted() {

		}
	}
	// - http://183.237.67.218:3000/search?keywords= 神话               搜索歌曲时接口获取音乐列表
	// - http://183.237.67.218:3000/song/url?id=310574                      获取音乐url
	// - http://183.237.67.218:3000/comment/music?id=310574         获取 用户评论列表
	// - http://183.237.67.218:3000/song/detail?ids=310574                获取 音乐详情   如图片，演唱者等
</script>

<style>
    body,
    ul,
    dl,
    dd {
        margin: 0px;
        padding: 0px;
    }

    .wrap {
        position: fixed;
        left: 0;
        top: 0;
        width: 100%;
        height: 100%;
        background: url('./assets/images/bg.jpg') no-repeat;
        background-size: 100% 100%;
    }

    .play_wrap {
        width: 800px;
        height: 544px;
        position: fixed;
        left: 50%;
        top: 50%;
        margin-left: -400px;
        margin-top: -272px;
        /*background-color: #f9f9f9;*/
    }

    .search_bar {
        height: 60px;
        background-color: #1eacda;
        overflow: hidden;
        border-top-left-radius: 4px;
        border-top-right-radius: 4px;
        display: flex;
        align-items: center;
        justify-content: space-between;
        position: relative;
        z-index: 11;
    }

    .search_bar img {
        margin-left: 23px;
    }

    .search_bar input {
        margin-right: 23px;
        width: 296px;
        height: 34px;
        border-radius: 17px;
        border: 0px;
        background: url('./assets/images/zoom.png') 265px center no-repeat rgba(255, 255, 255, 0.45);
        text-indent: 15px;
        outline: none;
    }

    .center_con {
        height: 435px;
        background-color: rgba(255, 255, 255, 0.5);
        display: flex;
    }

    .song_wrapper {
        width: 200px;
        height: 435px;
        box-sizing: border-box;
        padding: 10px 0;
        list-style: none;
        background: url('./assets/images/line.png') right center no-repeat;
        position: relative;
        overflow: hidden;
    }

    .song_list li {
        font-size: 12px;
        color: #333;
        line-height: 36px;
        width: 180px;
        overflow: hidden;
        text-overflow: ellipsis;
        white-space: nowrap;
        cursor: pointer;
        padding-left: 10px;
    }

    .song_list li:last-child {
        padding-bottom: 20px;
    }

    .song_list li.newli {
        background: url('./assets/images/zoom.png') 265px center no-repeat rgba(255, 255, 255, 0.45);
        color: #1eacda;
    }

    .song_list .active {
        color: #da651e;
    }

    .player_con {
        width: 400px;
        height: 435px;
        position: relative;
    }

    .disc {
        position: absolute;
        left: 73px;
        top: 60px;
        z-index: 9;
    }

    .cover {
        position: absolute;
        left: 125px;
        top: 112px;
        width: 150px;
        height: 150px;
        border-radius: 75px;
        z-index: 8;
    }

    .comment_list {
        width: 200px;
        height: 435px;
        box-sizing: border-box;
        padding: 10px;
        list-style: none;
        background: url('./assets/images/line.png') left center no-repeat;
        overflow: hidden;
        position: relative;
    }

    .comment_list dl {
        padding-left: 55px;
        position: relative;
        margin-bottom: 20px;
    }

    .comment_list dl:last-child {
        padding-bottom: 20px;
    }

    .comment_list dt {
        position: absolute;
        left: 4px;
        top: 0px;
    }

    .comment_list dt img {
        width: 40px;
        height: 40px;
        border-radius: 20px;
    }

    .comment_list dd {
        font-size: 12px;
    }

    .comment_list .name {
        font-weight: bold;
        color: #333;
        margin-top: 5px;
    }

    .comment_list .detail {
        color: #666;
        margin-top: 5px;
        line-height: 18px;
        -webkit-line-clamp: 2;
        display: -webkit-box;
        -webkit-box-orient: vertical;
        overflow: hidden;
        text-overflow: ellipsis;
    }

    .audio_con {
        height: 50px;
        background-color: #f1f3f4;
        border-bottom-left-radius: 4px;
        border-bottom-right-radius: 4px;
    }

    .myaudio {
        width: 800px;
        height: 40px;
        margin-top: 5px;
        outline: none;
        background-color: #f1f3f4;
    }

    /* 旋转的动画 */
    @keyframes Rotate {
        from {
            transform: rotateZ(0);
        }

        to {
            transform: rotateZ(360deg);
        }
    }

    /* 旋转的类名 */
    .autoRotate {
        animation-name: Rotate;
        animation-iteration-count: infinite;
        animation-play-state: paused;
        animation-timing-function: linear;
        animation-duration: 5s;
    }

    /* 是否正在播放 */
    .playing {
        animation-play-state: running;
    }

    .play_bar {
        position: absolute;
        left: 200px;
        top: -10px;
        z-index: 10;
        transform: rotate(-25deg);
        transform-origin: 12px 12px;
        transition: 1s;
    }

    /* 播放杆 转回去 */
    .play_bar.playing {
        transform: rotate(0);
    }
</style>

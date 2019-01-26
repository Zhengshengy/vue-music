<template lang="html">
  <div class="container-fluid" style="margin-bottom: 65px">
    <van-nav-bar title="小说详情" left-text="" left-arrow @click-left="$router.back(-1)"/>
    <div class="container">
      <!-- <van-row>
        <van-col class="my-15" span="16" offset="8">
          <p>{{novel.name}}</p>
        </van-col>
        <van-col span="16" offset="4">
          <img class="img-thumb img-fluid" :src="novel.poster" alt="">
        </van-col>
      </van-row> -->
      <audio ref="player" :src="audioHttp"></audio>
      <van-slider class="mt-30" v-model="value" active-color="#f44" @change="onChange">
        <div slot="button" class="custom-button photo-10 rounded bg-danger"></div>
      </van-slider>
      <div class="my-10 text-danger clearfloat">
        <span class="float-left">{{cTime}}</span>
        <span class="float-right">{{dTime}}</span>
      </div>
      <van-row type="flex" justify="center" align="center" guffer="20">
        <van-col span="4"><van-icon name="arrow-left" size="30px"/></van-col>
        <van-col span="6"><van-icon :name="play ? 'play' : 'stop'" size="50px" @click="audioState"/></van-col>
        <van-col span="2"><van-icon name="arrow" size="30px"/></van-col>
      </van-row>
      <!-- <van-row gutter="20" class="my-30">
        <van-col span="4">
          <img class="img-fluid rounded" src="../../../static/images/avatar/1.png" alt="">
        </van-col>
        <van-col span="14">
          <h3>完美情人</h3><p>个人认证、有声小说主播</p>
        </van-col>
        <van-col span="6">
          <van-button round type="danger" size="mini">关注</van-button>
        </van-col>
      </van-row> -->

      <h3 class="column-title">最新评论</h3>
     
    </div>
  </div>
</template>

<script>
  import { NavBar,Col,Row,Slider,Button,Icon,List,Toast} from 'vant';
  import CommentComponent from '@/components/Public/CommentComponent'
  import PublishComponent from '@/components/Public/PublishComponent'
  import axios from 'axios'
  export default {
    components:{
      CommentComponent,
      PublishComponent,
      [NavBar.name]:NavBar,
      [Col.name]:Col,
      [Row.name]:Row,
      [Slider.name]:Slider,
      [Button.name]:Button,
      [Icon.name]:Icon,
      [List.name]:List,
      [Toast.name]:Toast
    },
    created(){
      // this.$emit('public_header', false);
      // this.$emit('public_footer', false);
      // let id = this.$route.query.id
      // axios.get('static/data/comment.json').then(e=>{
      //   this.comments = e.data.filter(item=>{
      //     return item.post_id == '1'
      //   })
      // });
      // axios.get('static/data/novel.json').then(e=>{
      //   this.novel = e.data.filter(item=>{
      //     return item.id == id
      //   })
      //   this.novel = this.novel[0]
      // })
    },
    data() {
      return {
        value: 0,
        comments:[],
        novel:[],
        cTime:'00:00',
        dTime:'00:00',
        play: true,
        loading:false,
        finished:false,
        audioHttp: 'http://up.mcyt.net/?down/46426.mp3',
      }
    },
    mounted(){
      const music = this.$refs.player
      music.addEventListener('canplay', () => {
      const musicTime = music.duration
      const branch = Math.floor(musicTime / 60)
      const second = Math.ceil(musicTime % 60)
      if (branch < 10 && second < 10) {
        this.dTime = `0${branch}:0${second}`
      } else if (branch < 10) {
        this.dTime = `0${branch}:${second}`
      } else if (second < 10) {
        this.dTime = `${branch}:0${second}`
      } else {
        this.dTime = `${branch}:${second}`
      }
    })
      music.addEventListener('timeupdate', () => {
      const musicTime = music.duration
      const circleTime = musicTime / 100
      const stopTime = music.currentTime
      this.value =   stopTime/circleTime
      const branch = Math.floor(stopTime / 60)
      const second = Math.floor(stopTime % 60)
      if (branch < 10 && second < 10) {
        this.cTime = `0${branch}:0${second}`
      } else if (branch < 10) {
        this.cTime = `0${branch}:${second}`
      } else if (second < 10) {
        this.cTime = `${branch}:0${second}`
      } else {
        this.cTime = `${branch}:${second}`
      }
    })
    },
    methods: {
      onChange(value) {
        this.$toast('当前值：' + value);
        const music = this.$refs.player
        console.log(music.duration)
        music.currentTime = this.value * (music.duration/100)
        music.play()
        this.play = false
      },
      audioState () {
      this.play = !this.play // 更改播放暂停按钮状态
      const music = this.$refs.player // 音频所在对象
      if (this.play) {
        music.pause() // 播放音乐
      } else {
        music.play() // 暂停音乐
      }
    },

    }
  }
</script>

<style lang="css">
  .clearfloat{
    overflow: hidden;
  }
</style>

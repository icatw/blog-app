<template>
  <div v-title data-title="花花世界">
    <el-container>

      <el-main class="me-articles">

        <article-scroll-page></article-scroll-page>

      </el-main>

      <el-aside>

        <card-me class="me-area"></card-me>
        <card-tag :tags="hotTags"></card-tag>

        <card-article cardHeader="最热文章" :articles="hotArticles"></card-article>

        <card-archive cardHeader="文章归档" :archives="archives"></card-archive>

        <card-article cardHeader="最新文章" :articles="newArticles"></card-article>

      </el-aside>

    </el-container>

    <!-- 准备一个容器用来存放音乐播放器 -->
    <div id="aplayer"></div>

  </div>
</template>

<script>
import CardMe from '@/components/card/CardMe'
import CardArticle from '@/components/card/CardArticle'
import CardArchive from '@/components/card/CardArchive'
import CardTag from '@/components/card/CardTag'
import ArticleScrollPage from '@/views/common/ArticleScrollPage'

import {getArticles, getHotArtices, getNewArtices} from '@/api/article'
import {getHotTags} from '@/api/tag'
import {listArchives} from '@/api/article'

import APlayer from "APlayer"; // 引入音乐插件
import "APlayer/dist/APlayer.min.css"; // 引入音乐插件的样式


export default {
  name: 'Index',
  created() {

    this.getHotArtices()
    this.getNewArtices()
    this.getHotTags()
    this.listArchives()
  },
  data() {
    return {
      hotTags: [],
      hotArticles: [],
      newArticles: [],
      archives: [],
      audio: [ // 歌曲列表
        {
          name: "那是你离开了北京的生活", // 歌曲名字
          artist: "薛之谦", // 歌曲演唱者
          url: // 歌曲地址（这里用外链地址）
            "https://sharefs.ali.kugou.com/202203050319/e60d608a393fc9a318de54204ef50812/KGTX/CLTX001/045a49266170ec61b5eb77ca1e1f5836.mp3",
          cover: "https://p2.music.126.net/TOkRGd59o3hAOKsnMMmMMA==/109951163755246383.jpg", // 歌曲头像
          lrc: "../../static/lrc/那是你离开了北京的生活-薛之谦.lrc", // 歌词
          theme: "rgb(127, 218, 180)", // 播放这首歌曲时的主题色
        },
        {
          name: "哪里都是你",
          artist: "队长",
          url:
            "https://sharefs.ali.kugou.com/202203051017/40938031ed20565fc117c82b027ec884/G113/M01/00/15/UZQEAFmvs_GAWEvMADZhzt1fmGQ829.mp3",
          cover: "http://imge.kugou.com/stdmusic/150/20200606/20200606220631519630.jpg",
          lrc: "../../static/lrc/哪里都是你-队长.lrc",
          theme: "rgb(61, 162, 230)",
        },
        {
          name: "飞船失事",
          artist: "Tizzy T",
          url:
            "https://sharefs.ali.kugou.com/202203051019/dceab43916f27490bb3eab16b3ba4d88/KGTX/CLTX001/a9c159af53d7b051a575f4737683ec0e.mp3",
          cover: "https://p1.music.126.net/RIoutexhj0ssRt625Agh_w==/109951167035823879.jpg",
          lrc: "../../static/lrc/飞船失事-Tizzy T.lrc",
          theme: "#baf",
        },
      ],
      info: {
        lrcType: 3,
        fixed: false, // 不开启吸底模式
        listFolded: true, // 折叠歌曲列表
        autoplay: true, // 开启自动播放
        preload: "auto", // 自动预加载歌曲
        loop: "all", // 播放循环模式、all全部循环 one单曲循环 none只播放一次
        order: "list", //  播放模式，list列表播放, random随机播放
      },
    };
  },
  mounted() {
    // 初始化播放器
    this.initAudio();
  },
  methods: {
    initAudio() {
      // 创建一个音乐播放器实例，并挂载到DOM上，同时进行相关配置
      const ap = new APlayer({
        container: document.getElementById("aplayer"),
        audio: this.audio, // 音乐信息
        ...this.info, // 其他配置信息
      });
    },
    getHotArtices() {
      let that = this
      getHotArtices().then(data => {
        that.hotArticles = data.data
      }).catch(error => {
        if (error !== 'error') {
          that.$message({type: 'error', message: '最热文章加载失败!', showClose: true})
        }

      })

    },
    getNewArtices() {
      let that = this
      getNewArtices().then(data => {
        that.newArticles = data.data
      }).catch(error => {
        if (error !== 'error') {
          that.$message({type: 'error', message: '最新文章加载失败!', showClose: true})
        }

      })

    },
    getHotTags() {
      let that = this
      getHotTags().then(data => {
        that.hotTags = data.data
      }).catch(error => {
        if (error !== 'error') {
          that.$message({type: 'error', message: '最热标签加载失败!', showClose: true})
        }

      })
    },
    listArchives() {
      listArchives().then((data => {
        this.archives = data.data
      })).catch(error => {
        if (error !== 'error') {
          that.$message({type: 'error', message: '文章归档加载失败!', showClose: true})
        }
      })
    }

  },
  components: {
    'card-me': CardMe,
    'card-article': CardArticle,
    'card-tag': CardTag,
    ArticleScrollPage,
    CardArchive,
    APlayer
  }
}
</script>

<style scoped>

.el-container {
  width: 960px;
}

.el-aside {
  margin-left: 20px;
  width: 260px;
}

.el-main {
  padding: 0px;
  line-height: 16px;
}

.el-card {
  border-radius: 0;
}

.el-card:not(:first-child) {
  margin-top: 20px;
}
</style>

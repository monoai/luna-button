<template>
  <div id="app">
        <Modal></Modal>
        <nav class="navbar navbar-default navbar-fixed-top navbar-inner">
            <div class="container-fluid">
                <div class="navbar-header">
                    <button type="button" class="navbar-toggle collapsed" data-toggle="collapse" data-target="#bs-navbar-collapse" aria-expanded="false">
                        <span class="sr-only">{{ $t("action.toggleNavbar") }}</span>
                        <span class="icon-bar"></span>
                        <span class="icon-bar"></span>
                        <span class="icon-bar"></span>
                    </button>
                    <router-link class="navbar-brand" to="/">{{ $t("info.title") }}</router-link>
                </div>

                <div class="collapse navbar-collapse" id="bs-navbar-collapse">
                    <ul class="nav navbar-nav">
                        <li><a href="https://www.youtube.com/channel/UCa9Y57gfeY0Zro_noHRVrnw?sub_confirm=1" target="_blank"><img src="resources/youtube_pink3.png" height="18" style="vertical-align:middle"/>&nbsp;&nbsp;{{$t("info.yt_channel")}}</a></li>
                        <li><a href="https://twitter.com/himemoriluna" target="_blank"><img src="resources/twitter_pink.png" height="18" style="vertical-align:middle"/>&nbsp;&nbsp;{{$t("info.twitter")}}</a></li>
                        <li><a href="https://discord.gg/bXJ5fgm" target="_blank"><img src="resources/discord_pink.png" height="18" style="vertical-align:middle"/>&nbsp;&nbsp;{{$t("info.discord")}}</a></li>
                    </ul>
                    <ul class="nav navbar-nav navbar-right">
                        <li class="dropdown">
                            <a href="javascript:;" class="dropdown-toggle" data-toggle="dropdown" role="button" aria-haspopup="true" aria-expanded="false">{{$t("lang." + currentLang)}} <span class="caret"></span></a>
                            <ul class="dropdown-menu">
                                <li><a href="javascript:;" @click="chlang('en-US')">{{$t("lang.en-US")}}</a></li>
                                <li><a href="javascript:;" @click="chlang('zh-TW')">{{$t("lang.zh-TW")}}</a></li>
                                <li><a href="javascript:;" @click="chlang('ja-JP')">{{$t("lang.ja-JP")}}</a></li>
                                <li><a href="javascript:;" @click="chlang('ms-MY')">{{$t("lang.ms-MY")}}</a></li>
                            </ul>
                        </li>
                    </ul>
                </div>

            </div>
        </nav>
        <div class="container-fluid main-content">
            <router-view></router-view>
        </div>
        <footer class="footer">
            <div class="container-fluid footer-content">
                <div class="pull-right">
                    <div class="text-right"><a href="https://github.com/monoai/luna-button" target="_blank">{{$t("info.toGithub")}} <img src="https://img.shields.io/github/stars/monoai/luna-button.svg?style=social"/></a></div>
                    <div class="text-right">{{$t("info.notOfficial")}}</div>
                </div>
                <div>{{$t("info.tlHelpers")}}</div>
                <!--<div><p>Me testing out something</p></div>-->
                <div><a href="https://twitter.com/monoAI_" target="_blank"><span style="color: #000000">mono</span><span style="color: #FF0000">AI</span></a> 2020. <span style="color: rgba(0, 0, 0, 0.5)">Credits to zyzsdy for his <a href="https://aquaminato.moe/" target="_blank">Aqua Button.</a></span></div>
            </div>
        </footer>
  </div>
</template>

<style lang="scss">
@import "../node_modules/bootstrap/dist/css/bootstrap.css";
body{
    padding-top: 70px;
    background-image: url('/resources/body_bg.png');
}
.nav.navbar-nav li a, .navbar-default .navbar-brand{
    color: white;
    text-shadow: -1px 1px 0 #000,
                  1px 1px 0 #000,
                  1px -1px 0 #000,
                  -1px -1px 0 #000;
}
.navbar-default .navbar-brand:hover{
    color: #f2a8cd;
}
.navbar-default .navbar-brand:focus{
    color: #bf8ac2;
}
.nav.navbar-nav li a:hover{
    color: #f2a8cd;
}
.nav.navbar-nav li a:active,
.nav.navbar-nav li a:focus,
.nav.navbar-nav.navbar-right li a:focus{
    color: #bf8ac2;
}
.nav a:hover{
    border-bottom: 2px solid #998ede;
}
.navbar {
  min-height: 55px;
}
.navbar-inner{
    background-size: contain;
    background-image: url('/resources/navbar_bg.png');
    background-repeat: repeat-x;
    border-bottom: 3px solid #998ede;
}
.main-content{
    min-height: 100vh;
}
.footer {
    width: 100%;
    height: auto;
    background-color: #fecfdf;
    border-top: 3px solid #fdb3d8;
}
.footer-content {
    padding-top: 10px;
    color: #666;
}
.text-right{
    text-align: right;
}
</style>

<script>
import Vue from 'vue'
import Component from 'vue-class-component'
import Modal from './components/modal.vue'
//import fetchpost from './util/fetchpost'

@Component({
    components:{
        Modal
    }
})
class App extends Vue {
    get currentLang(){
        return this.$i18n.locale;
    }
    created(){
        //eslint-disable-next-line
        console.log("Nanora!");
        this.$i18n.locale = localStorage.getItem("lang") || this.$i18n.locale;
    }
    chlang(v){
        this.$i18n.locale = v;
        localStorage.setItem("lang", v);
    }
}

export default App;
</script>

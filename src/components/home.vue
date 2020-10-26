<template>
    <div class="container-fluid" >
        <div>
            <div class="cate-header">{{ $t("action.control")}}</div>
            <div class="cate-body">
                <button class="btn btn-info" @click="random">{{ $t("action.randomplay") }}</button>
                <button class="btn btn-info" @click="stopPlay">{{$t("action.stopvoice") }}</button>
                <button class="btn btn-info" :class="{ 'disabled': autoCheck || loopCheck}" @click="overlap" :title="$t('info.overlapTips')">
                    <input class="checkbox" type="checkbox" v-model="overlapCheck">
                    <span>{{ $t("action.overlap") }}</span>
                </button>
                <button class="btn btn-info" :class="{ 'disabled': overlapCheck || loopCheck }" @click="autoPlay">
                    <input class="checkbox" type="checkbox" v-model="autoCheck">
                    <span>{{ $t("action.autoplay") }}</span>
                </button>
                <button class="btn btn-info" :class="{ 'disabled': autoCheck || overlapCheck }" @click="loop" :title="$t('info.loopTips')">
                    <input class="checkbox" type="checkbox" v-model="loopCheck">
                    <span>{{ $t("action.loop") }}</span>
                </button>
            </div>
            <div class="cate-body">
                <span>{{ voice.name ? $t("action.playing") + $t("voice." + voice.name ) : $t("action.noplay") }}</span>
            </div>
            <audio id="player" @ended="voiceEnd(false)"></audio>
        </div>
        <div v-for="category in voices" v-bind:key="category.categoryName">
            <div class="cate-header">{{ $t("voicecategory." + category.categoryName) }}</div>
            <div class="cate-body">
                <button class="btn btn-new" v-for="voiceItem in category.voiceList" v-bind:key="voiceItem.name" @click="play(voiceItem)">
                    {{ $t("voice." + voiceItem.name )}}
                </button>
            </div>
        </div>
    </div>
</template>

<style lang="scss" scoped>
.cate-header{
    background-color: #dd2e44;
    color: white;
    text-shadow: 0 0 3px #FF0000, 0 0 1px #FF0000;
    border: 2px solid #ff546d;
    border-radius: 10px;
    text-align: center;
    font-size: 20px;
    margin-bottom: 12px;
}
.cate-body{
    margin-bottom: 12px;
    text-align: center;
}
.cate-body button.btn-info{
    background-color: white;
    background-repeat: repeat-x;
    background-size: contain;
    color: #c186a4;
    //text-shadow: -1px 1px 1px #666381,
    //              1px 1px 0px #666381,
    //              1px -1px 0 #666381,
    //              -1px -1px 0 #666381;
    border: 3px solid #dd2e44;
    border-radius: 20px;
    transition-duration: 0.4s;
    margin: 5px;
}
.btn-new {
    background-color: white;
    background-repeat: repeat-x;
    background-size: contain;
    color: #c186a4;
    //text-shadow: -1px 1px 1px #666381,
    //              1px 1px 0px #666381,
    //              1px -1px 0 #666381,
    //              -1px -1px 0 #666381;
    border: 3px solid #dd2e44;
    transition-duration: 0.4s;
    border-radius: 20px;
    margin: 5px;
    max-width: 100%;
    word-wrap: break-word !important;
    word-break: break-all !important;
    white-space: normal !important;
}
.cate-body button:hover{
    color: #ABA5D8;
    text-shadow: -1px 1px 1px #666381,
                  1px 1px 0px #666381,
                  1px -1px 0 #666381,
                  -1px -1px 0 #666381;
}
.checkbox {
    display: inline-block;
    vertical-align: middle;
    margin: 0 5px 3px 0;
}
</style>


<script>
import Vue from 'vue'
import Component from 'vue-class-component'
import VoiceList from '../voices.json'

@Component
class HomePage extends Vue {
    voices = VoiceList.voices;
    autoCheck = false;
    overlapCheck = false;
    loopCheck = false;
    currVoice;
    voice = {};

    play(item){
        if (this.overlapCheck) {
            let audio = new Audio("voices/" + item.path);
            this.voice = item;
            audio.play()
        } else {
            this.stopPlay();
            let player = document.getElementById('player');
            player.src = "voices/" + item.path;
            this.voice = item;
            this.currVoice = item;
            player.play();
        }
    }
    stopPlay(){
        let player = document.getElementById('player');
        player.pause();
        this.voiceEnd(true);
    }
    voiceEnd(flag) {
        if(flag !== true && this.autoCheck) {
            this.random();
        } else if(flag !== true && this.loopCheck) {
            let player = document.getElementById('player');
            player.play();
        } else {
            this.voice = {};
        }
    }
    random() {
        let tempList = this.voices[this._randomNum(0, this.voices.length - 1)];
        this.play(tempList.voiceList[this._randomNum(0, tempList.voiceList.length - 1)]);
    }
    autoPlay(){
        if (this.overlapCheck || this.loopCheck) {
            return;
        }
        this.autoCheck = !this.autoCheck;
    }
    overlap() {
        if (this.autoCheck || this.loopCheck) {
            return;
        }
        this.overlapCheck = !this.overlapCheck;
    }
    loop(){
        if (this.autoCheck || this.overlapCheck) {
            return;
        }
        this.loopCheck = !this.loopCheck;
    }
    _randomNum(minNum, maxNum) {
        switch(arguments.length) {
            case 1:
                return parseInt(Math.random() * minNum + 1, 10);
            case 2:
                return parseInt(Math.random() * (maxNum - minNum + 1) + minNum, 10);
            default:
                return 0;
        }
    }
}
export default HomePage;
</script>

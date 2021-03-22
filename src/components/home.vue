<template>
    <div class="container-fluid" >
        <div>
            <div class="cate-header">{{ $t("action.control")}}</div>
            <div class="cate-body">
                <button class="btn btn-info" @click="random">{{ $t("action.randomplay") }}</button>
                <button class="btn btn-info" @click="stopPlay">{{$t("action.stopvoice") }}</button>
                <button class="btn btn-info" :class="{ 'disabled': autoCheck || loopCheck}" @click="overlap" :title="$t('info.overlapTips')">
                    <input class="checkbox" type="checkbox" onchange="this.checked = this.parentNode.disabled" v-model="overlapCheck">
                    <span>{{ $t("action.overlap") }}</span>
                </button>
                <button class="btn btn-info" :class="{ 'disabled': overlapCheck || loopCheck }" @click="autoPlay">
                    <input class="checkbox" type="checkbox" onchange="this.checked = this.parentNode.disabled" v-model="autoCheck">
                    <span>{{ $t("action.autoplay") }}</span>
                </button>
                <button class="btn btn-info" :class="{ 'disabled': autoCheck || overlapCheck }" @click="loop" :title="$t('info.loopTips')">
                    <input class="checkbox" type="checkbox" onchange="this.checked = this.parentNode.disabled" v-model="loopCheck">
                    <span>{{ $t("action.loop") }}</span>
                </button>
                <div class="visible-md visible-lg">
                  <button class="btn btn-info">
                    <span>{{ $t("action.volume") }}</span>
                    <input ref="volSlider" class="slidecontainer slider" type="range" min="0" max="100" id="volSlider" v-model="currentVolume">
                  </button>
                  <p>{{ $t("action.volume") }}<span id="volOut">{{ currentVolume }}</span></p>
                </div>
            </div>
            <div class="cate-body">
                <span>{{ voice.name ? $t("action.playing") + $t("voice." + voice.name ) : $t("action.noplay") }}</span>
            </div>
            <audio ref="player" id="player" @ended="voiceEnd(false)"></audio>
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
.cate-body button.btn-info,
.btn-new{
    background-color: white;
    background-repeat: repeat-x;
    background-size: contain;
    color: #bf8ac2;
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
    max-width: 100%;
    word-wrap: break-word !important;
    word-break: break-all !important;
    white-space: normal !important;
}
.cate-body button:hover{
    color: #fdb3d8;
    text-shadow: -1px 0 1px #a358a8, 0 1px #a358a8, 1px 0 #a358a8, 0 -1px #a358a8;
}
.cate-body button:focus{
    color: #91d7f1;
    text-shadow: -1px 0 1px #666381, 0 1px #666381, 1px 0 #666381, 0 -1px #666381;
}
.checkbox {
    display: inline-block;
    vertical-align: middle;
    margin: 0 5px 3px 0;
}
/*Slider CSS modified from w3schools*/
.slider {
    -webkit-appearance: none;  /* Override default CSS styles */
    appearance: none;
    display: inline-block;
    width: auto; /* Full-width */
    height: 5px; /* Specified height */
    background: #d3d3d3; /* Grey background */
    outline: none; /* Remove outline */
    opacity: 0.7; /* Set transparency (for mouse-over effects on hover) */
    -webkit-transition: .2s; /* 0.2 seconds transition on hover */
    transition: opacity .2s;
}

/* Mouse-over effects */
.slider:hover {
    opacity: 1; /* Fully shown on mouse-over */
}

/* The slider handle (use -webkit- (Chrome, Opera, Safari, Edge) and -moz- (Firefox) to override default look) */
.slider::-webkit-slider-thumb {
    -webkit-appearance: none; /* Override default look */
    appearance: none;
    width: 15px; /* Set a specific slider handle width */
    height: 15px; /* Slider handle height */
    background: #4CAF50; /* Green background */
    cursor: pointer; /* Cursor on hover */
}

.slider::-moz-range-thumb {
    width: 15px; /* Set a specific slider handle width */
    height: 15px; /* Slider handle height */
    background: #4CAF50; /* Green background */
    cursor: pointer; /* Cursor on hover */
}
</style>

<script>
import Vue from 'vue'
import Component from 'vue-class-component'
import VoiceList from '../voices.json'

@Component({
    watch: {
        currentVolume: function (value) {
            this.$refs.player.volume = value / 100;
        }
    }
})
class HomePage extends Vue {
    voices = VoiceList.voices;
    autoCheck = false;
    overlapCheck = false;
    loopCheck = false;
    currVoice;
    voice = {};
    currentVolume = 80;

    get currentAudioVolume() {
        return  this.currentVolume / 100
    }
    mounted() {
        this.handlePlay = ({ src }) => {
            this.$refs.player.src = src
            this.$refs.player.play()
        }

        this.$gConst.globalbus.$on('play', this.handlePlay)
    }
    beforeDestroy() {
        this.$gConst.globalbus.$off('play', this.handlePlay)
    }
    play(item){
        if (this.overlapCheck) {
            let audio = new Audio("voices/" + item.path);
            audio.volume = this.currentAudioVolume;
            this.voice = item;
            audio.play()
        } else {
            this.stopPlay();
            this.$refs.player.src = "voices/" + item.path;
            this.voice = item;
            this.currVoice = item;
            this.$refs.player.play();
        }
    }
    stopPlay(){
        this.$refs.player.pause();
        this.voiceEnd(true);
    }
    voiceEnd(flag) {
        if(flag !== true && this.autoCheck) {
            this.random();
        } else if(flag !== true && this.loopCheck) {
            this.$refs.player.play();
        } else {
            this.voice = {};
        }
    }
    random() {
        let tempList = this.voices[this._randomNum(0, this.voices.length - 1)];
        if (tempList.categoryName == "blessings" ) {
            //eslint-disable-next-line
            console.log("BLESSING DENIED");
            this.random();
        } else {
            this.play(tempList.voiceList[this._randomNum(0, tempList.voiceList.length - 1)]);
        }
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

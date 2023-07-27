<script setup>
import { reactive, watch, onMounted } from 'vue'

/*---------------------------
    データ
---------------------------*/
const props = defineProps(["result", "campaign"])
const recommend = reactive({
    text: "",
    text2: "",
    imgSrc: "",
    homePage: "",
    twitter: "",
    ipPhone: "",
    option: "",
    optionPrice: 0,
    planPrice: 0,
    campaignPrice: 0,
    campaign: false
})

/*---------------------------
    メソッド
---------------------------*/
onMounted(() => {
    setRecommendData()
})
watch(props, () => {
    setRecommendData()
})

const setRecommendData = () => {
    const recommendSim = document.querySelector(".recommend-SIM")
    recommendSim.classList.remove("voiceSIM","SMS","dataeSIM","data")
    // 音声eSIMは2ギガプランの場合のみ文言が異なる
    if (props.result.sim === "音声eSIM" && props.result.dataVolume === 2) {
        recommend.text = "別キャリアの回線を持ちたい方"
        recommend.text2 = "万が一に備えて、スマホ1台で2回線持ちたい方"
        recommend.imgSrc = require("../assets/img/recommend_eSIM_2G.png")
        recommendSim.classList.add("voiceSIM")
    } else if (props.result.sim === "音声SIM" || props.result.sim === "音声eSIM") {
        switch (props.result.dataVolume) {
            case 2:
                recommend.text = "スマホをあまり利用しない方"
                recommend.text2 = "WI-Fi環境メインでスマホのデータ量が少ない方"
                recommend.imgSrc = require("../assets/img/recommend_2G.png")
                break;
            case 5:
                recommend.text = "通勤時間のネットやSNS利用が中心の方"
                recommend.text2 = "ご自宅ではWi-Fi利用されている方"
                recommend.imgSrc = require("../assets/img/recommend_5G.png")
                break;
            case 10:
                recommend.text = "音声通話が必要でデータをよく使う方"
                recommend.text2 = "ネットやSNSが中心でたまに動画を見る方"
                recommend.imgSrc = require("../assets/img/recommend_10G.png")
                break;
            case 15:
                recommend.text = "音声通話が必要でデータをたっぷり使いたい方"
                recommend.text2 = "外出先で動画視聴やゲームを楽しむことが多い方"
                recommend.imgSrc = require("../assets/img/recommend_15G.png")
                break;
            case 20:
                recommend.text = "データ容量を気にせず音声通話も使いたい方"
                recommend.text2 = "動画もゲームも頻繁に使われるヘビーユーザー"
                recommend.imgSrc = require("../assets/img/recommend_20G.png")
                break;
            default: break;
        }
        recommendSim.classList.add("voiceSIM")
    } else if (props.result.sim === "SMS") {
        switch (props.result.dataVolume) {
            case 2:
                recommend.text = "お子様のタブレットをお得に使い始めたい方"
                recommend.text2 = "メールやカンタンな調べ物が中心でお得に使いたい方"
                recommend.imgSrc = require("../assets/img/recommend_SMS_2G.png")
                break;
            case 5:
                recommend.text = "日常の連絡はSMSが中心で通話は行わない方"
                recommend.text2 = "ネットやSNS利用が中心の方"
                recommend.imgSrc = require("../assets/img/recommend_SMS_5G.png")
                break;
            case 10:
                recommend.text = "音声通話は使わずデータ容量にゆとりを持ちたい方"
                recommend.text2 = "ネットでの調べ物やゲームなどをよく使われる方"
                recommend.imgSrc = require("../assets/img/recommend_SMS_10G.png")
                break;
            case 15:
                recommend.text = "外出先などで動画視聴やゲームをする機会が多い方"
                recommend.text2 = "ゲームやSNSなど、電話番号認証を利用されたい方"
                recommend.imgSrc = require("../assets/img/recommend_SMS_15G.png")
                break;
            case 20:
                recommend.text = "データ容量を気にせず動画視聴やゲームを楽しむ方"
                recommend.text2 = "SNSやゲームなどの認証をスムーズに行いたい方"
                recommend.imgSrc = require("../assets/img/recommend_SMS_20G.png")
                break;
            default: break;
        }
        recommendSim.classList.add("SMS")
    } else if (props.result.sim === "データeSIM") {
        switch (props.result.dataVolume) {
            case 2:
                recommend.text = "eSIM対応端末での通信を試してみたい方"
                recommend.text2 = "自宅や職場でWi-Fi環境が利用できる方"
                recommend.imgSrc = require("../assets/img/recommend_dataeSIM_2G.png")
                break;
            case 5:
                recommend.text = "料金を抑えながらしっかりネットを使いたい方"
                recommend.text2 = "ネットやメールの送受信を頻繁に使われる方"
                recommend.imgSrc = require("../assets/img/recommend_dataeSIM_5G.png")
                break;
            case 10:
                recommend.text = "eSIM対応端末をできるだけお得に運用したい方"
                recommend.text2 = "ネットや動画といった用途で使われることが多い方"
                recommend.imgSrc = require("../assets/img/recommend_dataeSIM_10G.png")
                break;
            case 15:
                recommend.text = "eSIM対応端末でデータ通信をお得にしたい方"
                recommend.text2 = "外出先で動画視聴やゲームを楽しむ機会が多い方"
                recommend.imgSrc = require("../assets/img/recommend_dataeSIM_15G.png")
                break;
            case 20:
                recommend.text = "eSIM対応パソコンなどで利用したい方"
                recommend.text2 = "データ容量を気にせず動画やゲームを楽しみたい方"
                recommend.imgSrc = require("../assets/img/recommend_dataeSIM_20G.png")
                break;
            default: break;
        }
        recommendSim.classList.add("dataeSIM")
    } else if (props.result.sim === "データ") {
        switch (props.result.dataVolume) {
            case 2:
                recommend.text = "SIMフリー端末をできるだけお得に使い始めたい方"
                recommend.text2 = "費用を抑えてお子様にタブレットを持たせたい方"
                recommend.imgSrc = require("../assets/img/recommend_data_2G.png")
                break;
            case 5:
                recommend.text = "料金を抑えながらしっかりネットを使いたい方"
                recommend.text2 = "Web検索やメールの送受信を頻繁に使われる方"
                recommend.imgSrc = require("../assets/img/recommend_data_5G.png")
                break;
            case 10:
                recommend.text = "データのみ利用で、できるだけお得に使いたい方"
                recommend.text2 = "ネット検索やゲームプレイをよく楽しむ方"
                recommend.imgSrc = require("../assets/img/recommend_data_10G.png")
                break;
            case 15:
                recommend.text = "動画視聴もゲームも頻繁に楽しむアクティブな方"
                recommend.text2 = "複数台の端末をテザリングなどで接続したい方"
                recommend.imgSrc = require("../assets/img/recommend_data_15G.png")
                break;
            case 20:
                recommend.text = "スマホやパソコンとテザリングして使いたい方"
                recommend.text2 = "タブレット端末などで動画を楽しみたい方"
                recommend.imgSrc = require("../assets/img/recommend_data_20G.png")
                break;
            default: break;
        }
        recommendSim.classList.add("data")
    }
    // データの目安をセット(データ量*1GBあたりの目安)
    recommend.homePage = props.result.dataVolume * 3410
    recommend.twitter = props.result.dataVolume * 11250
    recommend.ipPhone = props.result.dataVolume * 29
    recommend.option = props.result.option
    recommend.optionPrice = props.result.optionPrice
    recommend.campaignPrice = props.result.campaignPrice
    recommend.planPrice = props.result.planPrice
    recommend.campaign = props.campaign
}
</script>

<template>
<div class="recommend-item">
    <div class="recommend-item-summery">
        <div class="yellowBlockNumber">{{ props.result.id }}</div>
        <div class="recommend-plan-icon"><img :src="props.result.simIconSrc"></div>
        <div class="recommend-plan">
            <div class="recommend-SIM">{{ props.result.sim }}</div>
            <div class="recommend-GB"><span>{{ props.result.dataVolume }}</span>GB</div>
            <div class="recommend-campaignPrice" v-if="recommend.campaign">
                <span class="recommend-price campaignPrice">月額<span>{{ props.result.totalPrice.toLocaleString() }}</span>円</span>
                <span class="normalPrice-total">{{ (recommend.optionPrice + recommend.planPrice).toLocaleString() }}円</span>
            </div>
            <div v-else class="recommend-price">月額<span>{{ props.result.totalPrice.toLocaleString() }}</span>円</div>
            <div class="recommend-option">
                <div  v-if="(props.result.sim === '音声SIM' || props.result.sim === '音声eSIM' ) && recommend.option !== ''">
                    定額通話<span>{{ recommend.option }}</span>+
                </div>
            </div>
        </div>
    </div>
    <div class="recommend-breakdownArea">
        <div class="recommend-breakdown">
            <p>内訳</p>
            <div class="recommend-plan-price">
                <div >
                    <div class="recommend-planName">{{ props.result.plan }}</div>
                    <div>税込<span class="result-price">{{ recommend.planPrice.toLocaleString() }}</span>円</div>
                </div>
                <div v-if="(props.result.sim === '音声SIM' || props.result.sim === '音声eSIM' ) && recommend.option !== ''">
                    <div>定額通話{{ recommend.option }}+</div>
                    <div v-if="recommend.campaign">
                        <span class="normalPrice">税込{{ recommend.optionPrice.toLocaleString() }}円</span><i class="fa fa-chevron-right" aria-hidden="true"></i>
                        <span class="campaignPrice">税込<span class="result-price">{{ recommend.campaignPrice.toLocaleString() }}</span>円</span>
                    </div>
                    <div v-else>税込
                        <span class="result-price">{{ recommend.optionPrice.toLocaleString() }}</span>円
                    </div>
                </div>
            </div>
            <small v-if="recommend.campaign">※通話定額割引はご利用開始月より6ヵ月間適用</small>
            <div v-if="recommend.campaign" class="campaign-detail">
                <p>適用キャンペーン</p>
                <p><a href=""><i class="fa fa-caret-square-o-right" aria-hidden="true"></i>サマーキャンペーン【通話オプション割引】詳細はこちら</a></p>
            </div>
        </div>
        <div class="recommend-detail">
            <p>こんな方にオススメ！</p>
            <div class="detail-flex">
                <div class="detail-flex_text">
                    <p><span class="point">&#9679;</span>{{ recommend.text }}</p>
                    <p><span class="point">&#9679;</span>{{ recommend.text2 }}</p>
                </div>
                <img :src="recommend.imgSrc">
            </div>
        </div>
    </div>
    <div class="recommend-text">
        <p>このデータ量の利用目安（{{ props.result.dataVolume }}GB）</p>
        <p>・ホームページ閲覧：約{{recommend.homePage.toLocaleString()}}回<br/>・Twitter：約{{ recommend.twitter.toLocaleString() }}回<br/>・IP電話：約{{ recommend.ipPhone.toLocaleString() }}時間<br/></p>
        <small>※1 ホームページ閲覧：Yahoo!トップページ（モバイル版）を表示／※2 Twitter：公式アプリで全角140文字でツイート後リロードする／※3 IP電話：Skypeを使用し通話<br/></small>
        <small>※弊社で独自に調査した結果であり、常にそのデータ量で利用できることを保証するものではありません。</small>
    </div>
</div>
</template>

<style scoped>
/*--------------------
    おすすめプラン表示
--------------------*/
.recommend-item {
    background-color: #fff;
    width: 96%;
    border: 1px solid #aaa;
    border-radius: 5px;
    margin: 0 auto 20px;
    padding: 8px;
}
.recommend-item-summery {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 0 15px;
    border-bottom: 1px solid #888;
}
.recommend-plan-icon>img{
    display: block;
    width: 40px;
    height: 40px;
    margin: 20px 10px 20px 20px;
}
.recommend-plan {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 20px;
    width: 100%;
}
.recommend-SIM {
    width: 25%;
    font-size: 2.6rem;
    font-weight: bold;
}
.voiceSIM {color: #c929b1;}
.dataeSIM {color: #29c9c6;}
.data {color: #2946c9;}
.SMS {color: #29c92c;}
.recommend-GB {
    width: 10%;
    text-align: right;
    font-size: 2.0rem;
    font-weight: bold;
}
.recommend-GB span {
    font-size: 3.6rem;
}

.recommend-campaignPrice {
    position: relative;
}
.recommend-price {
    width: 30%;
    font-size: 2.0rem;
    font-weight: bold;
}
.recommend-price span {
    font-size: 3.6rem;
}
.normalPrice-total {
    position: absolute;
    top: -20px;
    right: 0;
    font-size: 1.8rem;
    color: #b3b3b3;
    text-decoration: line-through;
}
.recommend-option {
    width: 30%;
    font-size: 2.0rem;
    font-weight: bold;
}
.recommend-option span {
    font-size: 2.6rem;
}
.recommend-breakdownArea {
    display: flex;
    padding: 10px 20px 0;
}
.recommend-breakdown {
    width: 55%;
    padding: 10px 15px 10px 0;
    border-right: 1px solid #888;
}
.recommend-breakdown>p {
    font-size: 1.8rem;
    font-weight: bold;
}
.recommend-breakdown>small {
    display: block;
    text-align: right;
    font-size: 1.2rem;
}
.fa-caret-square-o-right {
    margin-right: 5px;
}
.campaign-detail {
    background-color: #ffd9d9;
    margin: 15px 0;
    padding: 15px;
    font-size: 1.3rem;
}
.campaign-detail>p>a {
    color: #f93087;
}
.recommend-plan-price {
    border-top: 1px solid #888;
    border-bottom: 1px solid #888;
}
.recommend-plan-price>div {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 1.8rem;
    padding: 10px;
}
.recommend-planName {
    font-weight: bold;
}
.recommend-plan-price>div~div {
    border-top: 1px dotted #888;
}
.result-price {
    font-size: 2.4rem;
}
.normalPrice {
    font-size: 1.2rem;
    text-decoration: line-through;
}
.fa-chevron-right {
    font-size: 1.2rem;
}
.campaignPrice {
    position: relative;
    color: #f93087;
    font-weight: bold;
}
.campaignPrice::after {
    content: "※";
    position: absolute;
    color: #333;
    font-size: 1.2rem;
    bottom: 60%;
    right: 0;
}
.recommend-detail {
    width: 45%;
    padding: 10px;
    font-size: 1.2rem;
    line-height: 1;
}
.recommend-detail>p {
    font-size: 1.4rem;
    text-align: center;
    font-weight: bold;
    color: #ab8a11;
    padding: 10px;
}
.recommend-detail img {
    display: block;
    margin: 0 auto;
    width: 250px;
}
.point {
    color: #ab8a11;
    font-size: 1.8rem;
    margin-right: 3px;
}

.recommend-text {
    padding: 10px;
    font-size: 1.4rem;
}
.recommend-text p:first-child {
    font-weight: bold;
    color: #ab8a11;
}
.recommend-text>p>br {
    display: none;
}

@media screen and (max-width:768px) {

    .recommend-item-summery {
        padding: 10px 0;
    }
    .recommend-plan-icon>img{
        display: block;
        max-width: 30px;
        max-height: 30px;
        margin: 5px 0 5px 5px;
    }
    .recommend-plan {
        gap: 5px;
        justify-content: start;
    }
    .recommend-SIM {
        font-size: 1.2rem;
        width: auto;
    }
    .recommend-GB {
        font-size: 1.2rem;
        width: auto;
    }
    .recommend-GB span {
        font-size: 1.8rem;
    }
    .recommend-price {
        font-size: 1.2rem;
        width: auto;
    }
    .recommend-price span {
        font-size: 1.8rem;
    }
    .normalPrice-total {
        font-size: 1.2rem;
    }
    .recommend-option {
        font-size: 1.2rem;
        width: auto;
    }
    .recommend-option span {
        font-size: 1.8rem;
    }
    .recommend-breakdownArea {
        display: block;
        padding: 10px 0 0;
    }
    .recommend-breakdown {
        width: 100%;
    }
    .campaign-detail {
        font-size: 1.2rem;
    }
    .recommend-plan-price>div {
        font-size: 1.6rem;
    }
    .result-price {
        font-size: 1.8rem;
    }
    .recommend-detail {
        width: 100%;
    }
    .detail-flex {
        display: flex;
    }
    .detail-flex_text {
        font-size: 1.1rem;
    }
    .recommend-detail img {
        max-width: 200px;
    }
    .recommend-text {
        padding: 10px;
        font-size: 1.2rem;
    }
    .recommend-text>p>br {
        display: block;
    }
}

</style>

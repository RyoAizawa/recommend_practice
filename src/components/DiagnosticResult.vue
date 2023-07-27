<script setup>
import { ref, reactive, watch, defineEmits, computed, onUpdated } from 'vue'
import RecommendArea from "./RecommendArea";
import BreakDownItem from "./BreakDownItem";
/*---------------------------
    データ
---------------------------*/
const props = defineProps(["userAnswers", "lastBtnChecked", "autoScroll"])
const result = reactive({
    id: 0,
    lines: 0,
    sim: "",
    simIconSrc: "",
    plan: "",
    planPrice: 0,
    dataVolume: 0,
    option: "",
    optionPrice: 0,
    campaignPrice: 0,
    totalPrice: 0,
    campaign: false
})
const resultArray = reactive([])
const reverseResultArray = computed(() => {
    return resultArray.slice().reverse()
})
const count = computed(() => {
    let now = 0
    if(props.lastBtnChecked) now = 1
    return resultArray.length + 1 + now
})
const totalLines = computed(() => {
    let now = 0
    if(props.lastBtnChecked) now = 1
    return resultArray.length + now
})
const totalDataVolume = computed(() => {
    let now = 0
    let total = 0
    if(props.lastBtnChecked) now = result.dataVolume
    resultArray.forEach((elem) => {
        total += elem.dataVolume
    })
    return total + now
})
const totalPrice = computed(() => {
    let now = 0
    let total = 0
    if(props.lastBtnChecked) now = result.planPrice + result.campaignPrice
    resultArray.forEach((elem) => {
        total += elem.planPrice + elem.campaignPrice
    })
    return total + now
})
let campaignApply = ref(false)

/*---------------------------
    メソッド
---------------------------*/

const getLastBtnChecked = () => {
    return props.lastBtnChecked
}
const setResultData = () => {
    if (props.lastBtnChecked) {
        let voice = false
        let sms = false
        Object.values(props.userAnswers).forEach((elem) => {
            if (elem.questionId === 1) {
                if (elem.answerId === 1) {
                    result.sim = "音声SIM"
                    result.simIconSrc = require("../assets/img/voiceSIM.png")
                    voice = true
                } else {
                    voice = false
                }
            }
            // 通話ありが選択された
            if (voice) {
                if (elem.questionId === 2 && elem.answerId === 2) {
                    result.sim = "音声eSIM"
                    result.simIconSrc = require("../assets/img/voiceeSIM.png")
                }
                if (elem.questionId === 3) {
                    if (elem.answerId === 1) {
                        result.option = "5分"
                        result.optionPrice = 500
                        result.campaignPrice = 90
                        result.campaign = true
                    }
                    else if (elem.answerId === 2) {
                        result.option = "10分"
                        result.optionPrice = 700
                        result.campaignPrice = 290
                        result.campaign = true
                    }
                    else if (elem.answerId === 3) {
                        result.option = "かけ放題"
                        result.optionPrice = 1400
                        result.campaignPrice = 990
                        result.campaign = true
                    }
                    else result.option = ""
                }
            // 通話なしが選択された
            } else {
                if (elem.questionId === 4)
                    if (elem.answerId === 1) {
                        result.sim = "SMS"
                        result.simIconSrc = require("../assets/img/SMS.png")
                        sms = true
                    } else {
                        sms = false
                    }
                // smsなしが選択された
                if (!sms) {
                    if (elem.questionId === 5 && elem.answerId === 1) {
                        result.sim = "データeSIM"
                        result.simIconSrc = require("../assets/img/dataeSIM.png")
                    } else if (elem.questionId === 5 && elem.answerId === 2) {
                        result.sim = "データ"
                        result.simIconSrc = require("../assets/img/data.png")
                    }
                }
            }
            if (elem.questionId === 6) {
                if (elem.answerId === 1) result.dataVolume = 2
                else if (elem.answerId === 2) result.dataVolume = 5
                else if (elem.answerId === 3) result.dataVolume = 10
                else if (elem.answerId === 4) result.dataVolume = 15
                else if (elem.answerId === 5) result.dataVolume = 20
            }
        })
        calcPlanAndTotalPrice()
    }
}
const calcPlanAndTotalPrice = () => {
    result.plan = `${result.sim}${result.dataVolume}ギガプラン`
    switch (result.plan) {
        case "音声SIM2ギガプラン":     result.planPrice = 850;  break
        case "音声SIM5ギガプラン":     result.planPrice = 990;  break
        case "音声SIM10ギガプラン":    result.planPrice = 1500; break
        case "音声SIM15ギガプラン":    result.planPrice = 1800; break
        case "音声SIM20ギガプラン":    result.planPrice = 2000; break
        case "音声eSIM2ギガプラン":    result.planPrice = 850;  break
        case "音声eSIM5ギガプラン":    result.planPrice = 990;  break
        case "音声eSIM10ギガプラン":   result.planPrice = 1500; break
        case "音声eSIM15ギガプラン":   result.planPrice = 1800; break
        case "音声eSIM20ギガプラン":   result.planPrice = 2000; break
        case "SMS2ギガプラン":         result.planPrice = 820;  break
        case "SMS5ギガプラン":         result.planPrice = 920;  break
        case "SMS10ギガプラン":        result.planPrice = 1470; break
        case "SMS15ギガプラン":        result.planPrice = 1780; break
        case "SMS20ギガプラン":        result.planPrice = 1980; break
        case "データeSIM2ギガプラン":  result.planPrice = 440;  break
        case "データeSIM5ギガプラン":  result.planPrice = 660;  break
        case "データeSIM10ギガプラン": result.planPrice = 1100; break
        case "データeSIM15ギガプラン": result.planPrice = 1430; break
        case "データeSIM20ギガプラン": result.planPrice = 1650; break
        case "データ2ギガプラン":      result.planPrice = 740;  break
        case "データ5ギガプラン":      result.planPrice = 900;  break
        case "データ10ギガプラン":     result.planPrice = 1400; break
        case "データ15ギガプラン":     result.planPrice = 1730; break
        case "データ20ギガプラン":     result.planPrice = 1950; break
        default: break;
    }
    result.totalPrice = result.planPrice + result.campaignPrice
}
watch(getLastBtnChecked, () => {
    if (props.lastBtnChecked) {
        props.autoScroll(document.querySelector(".result-area"))
        result.id = resultArray.length + 1
        result.lines = resultArray.length + 1
        setResultData()
    }
})
watch(props.userAnswers, () => {
    result.option = ""
    result.campaign = false
    setResultData()
    checkCampaign()
})

const checkCampaign = () => {
    campaignApply.value = false
    if(result.option !== "") campaignApply.value  = true
    resultArray.forEach((elem) => {
        if(elem.option !== "") campaignApply.value = true
    })
}

const emit = defineEmits(["init"])
const addItem = () => {
    if (props.lastBtnChecked) {
        resultArray.push(Object.assign({},result))
    }
    emit("init");
}
const deleteItemNow = () => {
    initialize()
    checkCampaign()

    if (resultArray.length < 1) {
        const accordionArea = document.querySelector(".result-breakdownArea")
        accordionArea.classList.remove("active");
        breakDownBtnRotate()
    }

    emit("init");
}
const deleteItem = (target) => {
    resultArray.splice(target, 1)
    resultArray.forEach((elem) => {
        if(elem.id > target) elem.id--
    })
    result.id--
    checkCampaign()
    if (resultArray.length < 1) {
        const accordionArea = document.querySelector(".result-breakdownArea")
        accordionArea.classList.remove("active");
        breakDownBtnRotate()
    }
}
const deleteAllItem = () => {
    initialize()
    checkCampaign()
    resultArray.splice(0)
    const accordionArea = document.querySelector(".result-breakdownArea")
    accordionArea.classList.remove("active");
    breakDownBtnRotate()
    emit("init");
}
const initialize = () => {
    result.id = 0
    result.lines = 0
    result.sim = ""
    result.simIconSrc = ""
    result.plan = ""
    result.planPrice = 0
    result.dataVolume = 0
    result.option = ""
    result.optionPrice = 0
    result.totalPrice = 0
}

const breakdownBtnClick = () => {
    const accordionArea = document.querySelector(".result-breakdownArea")
    accordionArea.classList.toggle("active");
    breakDownBtnRotate()
    calcAccordionArea()
}

const breakDownBtnRotate = () => {
    const accordionArea = document.querySelector(".result-breakdownArea")
    if (accordionArea.classList.contains("active")) {
        document.querySelector(".fa-angle-down").animate(
            [
            { transform: 'rotate(0deg)' },
            { transform: 'rotate(180deg)' }
        ],
        {
            duration: 200,
            fill: "forwards",
            easing: 'ease-in',
        })
    } else {
        document.querySelector(".fa-angle-down").animate(
            [
            { transform: 'rotate(180deg)' },
            { transform: 'rotate(0deg)' }
        ],
        {
            duration: 200,
            fill: "forwards",
            easing: 'ease-in',
        })
    }
}


const calcAccordionArea = () => {
    const accordionArea = document.querySelector(".result-breakdownArea")
    const accordionItem = document.querySelectorAll(".result-breakdown-item")
    let totalHeight = 0
    console.log(accordionItem)
    accordionItem.forEach((elem) => {
        totalHeight += elem.scrollHeight
    });
    if (accordionArea.classList.contains("active")) {
        accordionArea.style.setProperty("max-height", totalHeight + 30 + "px");
    } else {
        accordionArea.style.setProperty("max-height", 0);
    }
}

onUpdated(() => {
    if(resultArray.length > 0) calcAccordionArea()
})

</script>

<template>
    <div class="result-area">
        <div class="campaign">通話定額が6ヵ月間割引！さらに初期費用割引中（8/31まで）！</div>
        <h2>
            <i class="fa fa-file-text-o" aria-hidden="true"></i
            >診断結果
        </h2>
        <div class="result-calcArea">
            <p>診断結果<span>合計</span></p>
            <div class="result-calcArea-right">
                <div class="result-whiteArea">
                    <div v-if="campaignApply && (props.lastBtnChecked || resultArray.length > 0)" class="campaign-apply">キャンペーン適用で</div>
                    <div class="result-firstArea">
                        <div>回線数
                            <div v-if="totalLines < 1">
                                <span>-</span>回線
                            </div>
                            <div v-else>
                                <span>{{ totalLines }}</span>回線
                            </div>
                        </div>
                        <div>データ容量
                            <div v-if="totalDataVolume < 1">
                                <span>-</span>GB
                            </div>
                            <div v-else>
                                <span>{{ totalDataVolume }}</span>GB
                            </div>
                        </div>
                        <div>月額
                            <div v-if="totalPrice < 1">
                                <span>-</span>円
                            </div>
                            <div v-else :class="{ campaignPrice : campaignApply }">
                                <span>{{ totalPrice.toLocaleString() }}</span>円
                            </div>
                        </div>
                        <div class="breakdownBtn-Wrap">
                            <button class="yellowBtn breakdownBtn"
                            @click="breakdownBtnClick"
                            :class="{disabled: !props.lastBtnChecked && !resultArray.length > 0}"
                            :disabled=" !props.lastBtnChecked && !resultArray.length > 0">
                                <i class="fa fa-angle-down" aria-hidden="true"></i>内訳
                            </button>
                        </div>
                    </div>
                    <!-- 内訳を押すと出力 -->
                    <div v-if="resultArray.length > 0 || props.lastBtnChecked" class="result-breakdownArea" :style="{maxHeight: 0}">
                        <div>
                            <div class="result-breakdownTable">
                                <div class="result-breakdown-label">
                                    内訳
                                </div>
                                <div class="result-breakdown-items">
                                    <BreakDownItem v-if="props.lastBtnChecked" :result="result" @deleteItem="deleteItemNow" :campaign="result.campaign" class="result-breakdown-item" />
                                    <BreakDownItem v-for="resultItem of reverseResultArray" :key="resultItem.id" :result="resultItem" @deleteItem="deleteItem" :campaign="resultItem.campaign" class="result-breakdown-item" />
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <small v-if="campaignApply">※通話定額割引はご利用開始月より6ヵ月間適用</small>
            </div>
        </div>
    </div>
    <!-- おすすめプラン表示 -->
    <div v-if="resultArray.length > 0 || props.lastBtnChecked" class="recommend">
        <h3>あなたに最適なプランはこれ！</h3>
        <button @click="deleteAllItem"  class="deleteBtn"><i class="fa fa-times" aria-hidden="true"></i>診断結果をすべて削除</button>
        <RecommendArea v-if="props.lastBtnChecked" :result="result" :campaign="result.campaign" />
        <RecommendArea v-for="resultItem of reverseResultArray" :key="resultItem.id" :result="resultItem" :campaign="resultItem.campaign" />
        <button
            @click="addItem"
            class="recommend-addBtn yellowBtn">
            <i class="fa fa-plus" aria-hidden="true"></i>
            {{ count }}人目を診断する
        </button>
        <button class="recommend-buyBtn"><i class="fa fa-chevron-right" aria-hidden="true"></i>ご購入・お申し込みはこちら</button>
    </div>
</template>

<style scoped>
/*--------------------
    診断結果領域
--------------------*/
.result-area {
    position: relative;
    background-color: #a5750c;
    margin-top: 100px;
    color: #fff;
    padding: 0 15px 10px;
}
.campaign {
    position: absolute;
    text-align: center;
    background-color: #f93087;
    width: 60%;
    padding: 5px;
    top: -60px;
    left: 50%;
    transform: translateX(-50%);
}
.campaign::after {
    position: absolute;
    content: "";
    border-top: 20px solid #f93087;
    border-right: 20px solid transparent;
    border-left: 20px solid transparent;
    border-bottom: 20px solid transparent;
    bottom: -100%;
    left: 50%;
    transform: translateX(-50%);
}
.campaign-apply {
    background-color: #f93087;
    color: #fff;
    text-align: center;
    width: 70%;
    margin: 10px 20px;
    border-radius: 5px;
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

.result-area > h2 {
    text-align: center;
    font-size: 2.8rem;
    padding: 5px;
    margin-bottom: 10px;
    letter-spacing: 7px;
    border-bottom: 1px solid #fff;
}
.fa-file-text-o {
    margin-right: 7px;
}
.result-calcArea {
    display: flex;
    flex-wrap: wrap;
}
.result-calcArea > p {
    text-align: center;
    width: 15%;
    margin: 0;
}
.result-calcArea > p > span {
    display: block;
    font-size: 2.2rem;
}
.result-calcArea-right {
    width: 85%;
}
.result-whiteArea {
    position: relative;
    width: 100%;
    color: #333;
    font-weight: bold;
    background-color: #fff;
    overflow:hidden;
}
.result-firstArea {
    display: flex;
    justify-content: space-between;
}
.result-firstArea>div {
    display: flex;
    align-items: baseline;
    font-size: 1.8rem;
    padding: 0 10px;
    margin: 10px;
}
.result-firstArea span {
    margin: 0 10px
}
.result-firstArea div:nth-of-type(2),
.result-firstArea div:nth-of-type(3) {
    border-left: 1px solid #333;

}
.result-firstArea span {
    font-size: 2.8rem;
}
.breakdownBtn-Wrap {
    position: relative;
    width: 15%;
}
.breakdownBtn {
    position: absolute;
    top: 50%;
    right: 20px;
    transform: translateY(-50%);
    display: flex;
    align-items: center;
    padding: 10px;
}
.fa-angle-down {
    font-size: 2.4rem;
}
/*--------------------
    内訳
--------------------*/
.result-breakdownArea {
    transition: max-height 0.3s;
    overflow: hidden;
    max-height: 0;
}
.result-breakdownArea>div {
    padding: 15px;
}
.active {
    max-height: var(--max-height);
}

.result-breakdownTable {
    display: flex;
    width: 100%;
    border-top: 1px solid #ab8a11;
    border-bottom: 1px solid #ab8a11;
}

.result-breakdown-label {
    display: flex;
    justify-content: center;
    align-items: center;
    color: #846a0e;
    width: 10%;
    background-color: #f4e5af;
}
.result-breakdown-items {
    width: 100%;
}

/*--------------------
    おすすめプラン表示領域
--------------------*/
.recommend {
    position: relative;
    background-color: #f2eacc;
    border: 1px solid #ab8a11;
}
.recommend>h3 {
    text-align: center;
    padding: 20px;
}
.recommend>.deleteBtn {
    position: absolute;
    top: 20px;
    right: 20px;
}

.recommend-addBtn {
    display: block;
    margin: 20px auto;
}
.recommend-buyBtn {
    display: block;
    margin: 20px auto;
    padding: 15px 30px;
    color: #fff;
    border: none;
    border-radius: 5px;
    box-shadow: 2px 2px #437c3d;
    background: linear-gradient(-20deg, #529d34 0%, #a8ff66 100%);
}
.fa-chevron-right {
    margin-right: 40px;
}
</style>

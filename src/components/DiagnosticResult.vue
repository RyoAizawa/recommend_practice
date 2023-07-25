<script setup>
import { reactive, watch, defineEmits, computed } from 'vue'
import RecommendArea from "./RecommendArea";
import BreakDownItem from "./BreakDownItem";
/*---------------------------
    データ
---------------------------*/
const props = defineProps(["userAnswers", "lastBtnChecked"])
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
    totalPrice: 0,
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
/*---------------------------
    メソッド
---------------------------*/
const breakdownBtnClick = () => {
    document.querySelector(".result-breakdownArea").classList.toggle("hide")
}
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
                        result.optionPrice = 90
                    }
                    else if (elem.answerId === 2) {
                        result.option = "10分"
                        result.optionPrice = 290
                    }
                    else if (elem.answerId === 3) {
                        result.option = "かけ放題"
                        result.optionPrice = 990
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
    result.totalPrice = result.planPrice + result.optionPrice
}
watch(getLastBtnChecked, () => {
    if (props.lastBtnChecked) {
        result.id = resultArray.length + 1
        result.lines = resultArray.length + 1
        setResultData()
    }
})
watch(props.userAnswers, () => {
    setResultData()
})

const emit = defineEmits(["init"])
const addItem = () => {
    if (props.lastBtnChecked) {
        resultArray.push(Object.assign({},result))
    }
    emit("init");
}
const deleteItemNow = () => {
    initialize()
    emit("init");
}
const deleteItem = (target) => {
    resultArray.splice(target, 1)
    resultArray.forEach((elem) => {
        if(elem.id > target) elem.id--
    })
    result.id--
}
const deleteAllItem = () => {
    initialize()
    resultArray.splice(0)
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

</script>

<template>
    <div class="result-area">
        <h2>
            <i class="fa fa-file-text-o" aria-hidden="true"></i
            >診断結果
        </h2>
        <div class="result-calcArea">
            <p>診断結果<span>合計</span></p>
            <div class="result-calcArea-right">
                <div class="result-whiteArea">
                    <div class="result-firstArea">
                        <div>回線数
                            <span v-if="result.lines < 1">-</span>
                            <span v-else>{{ result.lines }}</span>
                            回線
                        </div>
                        <div>データ容量
                            <span v-if="result.dataVolume < 1">-</span>
                            <span v-else>{{ result.dataVolume }}</span>
                            GB
                        </div>
                        <div>月額
                            <span v-if="result.totalPrice < 1">-</span>
                            <span v-else>{{ result.totalPrice.toLocaleString()  }}</span>
                            円
                        </div>
                        <button class="yellowBtn breakdownBtn"
                        @click="breakdownBtnClick"
                        :class="{disabled: !props.lastBtnChecked && !resultArray.length > 0}"
                        :disabled=" !props.lastBtnChecked && !resultArray.length > 0">
                            <i class="fa fa-angle-down" aria-hidden="true"></i>内訳
                        </button>
                    </div>
                    <!-- 内訳を押すと出力 -->
                    <div v-if="resultArray.length > 0 || props.lastBtnChecked" class="result-breakdownArea hide">
                        <div class="result-breakdownTable">
                            <div class="result-breakdown-label">
                                内訳
                            </div>
                            <div class="result-breakdown-items">
                                <BreakDownItem v-if="props.lastBtnChecked" :result="result" @deleteItem="deleteItemNow" />
                                <BreakDownItem v-for="resultItem of reverseResultArray" :key="resultItem.id" :result="resultItem" @deleteItem="deleteItem" />
                            </div>
                        </div>
                    </div>
                </div>
                <small>※通話定額割引はご利用開始月より6ヵ月間適用</small>
            </div>
        </div>
    </div>
    <!-- おすすめプラン表示 -->
    <div v-if="resultArray.length > 0 || props.lastBtnChecked" class="recommend">
        <h3>あなたに最適なプランはこれ！</h3>
        <button @click="deleteAllItem"  class="deleteBtn"><i class="fa fa-times" aria-hidden="true"></i>診断結果をすべて削除</button>
        <RecommendArea v-if="props.lastBtnChecked" :result="result"></RecommendArea>
        <RecommendArea v-for="resultItem of reverseResultArray" :key="resultItem.id" :result="resultItem"></RecommendArea>
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
    background-color: #ab8a11;
    margin-top: 100px;
    color: #fff;
    padding: 0 15px 10px;
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
    justify-content: space-between;
    color: #333;
    font-weight: bold;
    background-color: #fff;
}
.result-firstArea {
    display: flex;
}
.result-firstArea div {
    font-size: 1.8rem;
    padding: 0 10px;
    margin: 10px;
    width: 27%;
}
.result-firstArea div:nth-of-type(2),
.result-firstArea div:nth-of-type(3) {
    border-left: 1px solid #333;
}
.result-firstArea span {
    font-size: 2.8rem;
}
.breakdownBtn {
    position: absolute;
    top: 10px;
    right: 20px;
}

/*--------------------
    内訳
--------------------*/
.result-breakdownArea {
    padding: 15px;
}
.hide {
    display: none;
    animation-duration: 1.5s;
    animation-name: fade-in;
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

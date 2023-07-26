<script setup>
import DiagnosticResult from "./DiagnosticResult";
import "vue3-carousel/dist/carousel.css";
import { Carousel, Slide } from "vue3-carousel";
import { ref , reactive} from 'vue'

/*---------------------------
    データ
---------------------------*/
// 質問の内容と選択肢が格納されたデータ
const questionArray = [
    {
        id: 1,
        question: "電話番号（090等）は必要ですか？",
        answerArray: [
            {
                answerId: 1,
                image: require("../assets/img/1-1.png"),
                mainText: "必要",
                subText:"（お乗り換えの方はこちら）",
            },
            {
                answerId: 2,
                image: require("../assets/img/1-2.png"),
                mainText: "不要",
            }
        ]
    },
    {
        id: 2,
        question: "ご利用になるSIMの形状を教えてください",
        answerArray: [
            {
                answerId: 1,
                image: require("../assets/img/2-1.png"),
                mainText: "SIMカード",
                subText: "（カード型）",
            },
            {
                answerId: 2,
                image: require("../assets/img/2-1.png"),
                mainText: "eSIM*",
            }
        ],
        desc: "※eSIMご利用にはeSIM対応機種が必要です。",
        subDesc: "未対応の場合はご利用できませんので、「SIMカード」を選択してください。",
        descLink: require("../assets/img/descLink_eSIM.png"),
    },
    {
        id: 3,
        question: "通話定額を使いますか？",
        answerArray: [
            {
                answerId: 1,
                image: require("../assets/img/3-1.png"),
                mainText: "通話定額5分＋",
                subText: "1回5分以内の国内通話無料",
                optionPrice: "500",
                campaignPrice: "90",
                bandMsg: "セール"
            },
            {
                answerId: 2,
                image: require("../assets/img/3-2.png"),
                mainText: "通話定額10分＋",
                subText: "1回10分以内の国内通話無料",
                optionPrice: "700",
                campaignPrice: "290",
                bandMsg: "セール"
            },
            {
                answerId: 3,
                image: require("../assets/img/3-3.png"),
                mainText: "かけ放題＋",
                subText: "無制限で国内通話無料",
                optionPrice: "1400",
                campaignPrice: "990",
                bandMsg: "セール"
            },
            {
                answerId: 4,
                image: require("../assets/img/3-4.png"),
                mainText: "通話定額は使わない",
                subText: "",
                optionPrice: "",
                campaignPrice: "",
                bandMsg: ""
            },
        ]
    },
    {
        id: 4,
        question: "SMSを使いますか？",
        answerArray: [
            {
                answerId: 1,
                image: require("../assets/img/4-1.png"),
                mainText: "SMSを使う",
            },
            {
                answerId: 2,
                image: require("../assets/img/4-2.png"),
                mainText: "SMSは使わない",
            },
        ]
    },
    {
        id: 5,
        question: "データeSIMを使いますか？",
        answerArray: [
            {
                answerId: 1,
                image: require("../assets/img/5-1.png"),
                mainText: "データeSIMを使う",
            },
            {
                answerId: 2,
                image: require("../assets/img/5-2.png"),
                mainText: "データeSMSは使わない",
            },
        ],
        descLink: require("../assets/img/descLink_dataeSIM.png"),
    },
    {
        id: 6,
        question: "データ通信は毎月どのくらいご利用ですか？",
        answerArray: [
            {
                answerId: 1,
                image: require("../assets/img/6-1.png"),
                mainText: "家のWi-Fi利用が中心\n外出先でネットはあまり使わない",
            },
            {
                answerId: 2,
                image: require("../assets/img/6-2.png"),
                mainText: "外出先のSNS利用やネット閲覧が中心",
                bandMsg: "オススメ!"
            },
            {
                answerId: 3,
                image: require("../assets/img/6-3.png"),
                mainText: "外出先でゲームアプリを使う",
            },
            {
                answerId: 4,
                image: require("../assets/img/6-4.png"),
                mainText: "外出先で動画を視聴しネットも良く使う",
            },
            {
                answerId: 5,
                image: require("../assets/img/6-5.png"),
                mainText: "外出先でギガ（データ量）を気にせずに使いたい",
            },
        ]
    },
]
const myCarousel = ref(null)         // カルーセル
const currentSlide = ref(0)          // カルーセルの現在のスライド位置
const userAnswers = reactive([])     // ユーザーが選択した回答を格納するデータ
const userAnswersFlow = reactive([]) // カルーセルの移動用にユーザーが選択したデータの順番を保持
let isLastBtnChecked = ref(false)    // ユーザーが最後（6番目）の選択肢を選択したか判定
const isBackBtnActive = ref(false)   // 前の設問にもどるボタンがアクティブか判定
Carousel.props.mouseDrag = false
/*---------------------------
    メソッド
---------------------------*/
/*
    最後の選択肢が選択されたかチェックするメソッド
        questionId ... 回答した質問番号
*/
const lastBntChecked = (questionId) => {
    if(questionId === 6) isLastBtnChecked.value = true
}

/*
    回答ボタンを押した際に走る。
    1-1.質問番号に応じた回答内容をデータに格納
    1-2.回答した内容に応じたカルーセルのスライドを実行するメソッドを呼ぶ
    2.選択した際にボタンの色を変更するクラスを付与&削除
        currentSlide        ... 現在表示中のスライド位置
        questionArray       ... 質問の内容と選択肢が格納されたデータ
        currentQuestionId   ... 今回回答した質問番号
        answer              ... 今回回答した回答番号
        currentTarget       ... 現在のスライド位置
*/
const btnClick = (currentSlide, questionArray, currentQuestionId, answer, currentTarget) => {
    // 1.質問番号に応じた回答内容をデータに格納
    const currentAnswer = {}
    let overwrite = false
    // ユーザーが回答した内容を走査
    userAnswers.forEach((answered) => {
        // 今回選択した回答が回答済みの場合
        if (answered.questionId === currentQuestionId) {
            // 今回の回答に上書き
            answered.answerId = answer.answerId
            overwrite = true
            // 2-2.回答した内容に応じたカルーセルのスライドを実行するメソッドを呼ぶ
            carouselForeword(answered, currentSlide)
        }
    })
    // ユーザー回答がまだ1つもされていない場合、新規の回答の場合今回の回答をプッシュ
    if (userAnswers.length < 1 || !overwrite) {
        currentAnswer.questionId = currentQuestionId
        currentAnswer.answerId = answer.answerId
        userAnswers.push(currentAnswer)
        carouselForeword(currentAnswer, currentSlide)
    }

    // 2.選択した際にボタンの色を変更するクラスを付与&削除
    // 選択したボタンの色を変更するために都度状態を見る必要がある
    const answerBtnAll = document.querySelectorAll(".simulator-answerBtn")
    const currentQuestions = []     // 今回の質問に対する選択肢のノードを格納する配列
    let btnIndex = 0;
    questionArray.forEach((question) => {
        // 質問番号内容を捜査し、現在の質問番号と一致するか
        if (question.id === currentQuestionId) {
            // 一致したら全回答ボタンの中から、今回の質問に対する選択肢のボタンを抽出
            for (let i = 0; i < question.answerArray.length; i++) {
                currentQuestions.push(answerBtnAll[btnIndex + i])
            }
        } else {
            // 一致しなければインデックスにその質問の選択肢分の数を足す
            btnIndex += question.answerArray.length
        }
    })
    // 今回の質問に対する選択肢ボタンを走査
    currentQuestions.forEach((elem) => {
        // 今回の質問に対する選択済みクラスを一旦初期化
        elem.classList.remove("answered", "notAnswered")
        const children= elem.children
        for (let child of children) {
            child.classList.remove("selectedMainText")
        }
        // 今回選択した選択肢に選択済みのクラスを付与
        if (elem === currentTarget) {
            elem.classList.add("answered")
            const children= elem.children
            for (let child of children) {
                if (child.classList.contains("answerBtn-mainText")) {
                    child.classList.add("selectedMainText")
                }
            }
        } else {
            elem.classList.add("notAnswered")
        }
    })
}

/*
    「前の設問にもどる」ボタンを押した際にスライド位置を戻すメソッド
        currentSlide ... 現在表示中のスライド位置
*/
const carouselBack = (target) => {
    const previousTarget = userAnswersFlow[userAnswersFlow.length - 1].questionId - 1
    myCarousel.value.slideTo(previousTarget);
    // 一時的に保存していた配列データの末尾を削除
    userAnswersFlow.pop()
    target.disabled = true
    window.setTimeout(() => {
        target.disabled = false
        if(userAnswersFlow.length < 1) isBackBtnActive.value = false
    }, 500);
}

/*
    選択肢ボタンを押した際にスライド位置を進めるメソッド
        answered     ... 今回回答した回答内容
        currentSlide ... 現在表示中のスライド位置
*/
const carouselForeword = (answered, currentSlide) => {
    let slideValue = 0 // カルーセルの移動量
    if ((answered.questionId === 1 && answered.answerId === 2) || answered.questionId === 3) {
        slideValue = 3
    } else if (answered.questionId === 4 && answered.answerId === 1) {
        slideValue = 2
    } else {
        slideValue = 1
    }

    // 現在の位置から指定したページ分進める
    myCarousel.value.slideTo((currentSlide.currentSlide + slideValue));
    if (currentSlide.currentSlide < 5) userAnswersFlow.push(answered)
    if (userAnswersFlow.length > 0) isBackBtnActive.value = true
}

const initialize = () => {
    const target = document.querySelector(".simulator-main");
    autoScroll(target)
    myCarousel.value.slideTo(0);
    userAnswers.splice(0)
    userAnswersFlow.splice(0)
    isBackBtnActive.value = false
    isLastBtnChecked.value = false

    const answerBtnAll = document.querySelectorAll(".simulator-answerBtn")
    answerBtnAll.forEach((elem) => {
        elem.classList.remove("answered", "notAnswered")
        const children= elem.children
        for (let child of children) {
            child.classList.remove("selectedMainText")
        }
    })
}

const autoScroll = (target) => {
    let rect = target.getBoundingClientRect();
    // 要素の頂点の高さを設定
    let position = rect.top + scrollY;
    setTimeout(() => {
        scroll(0, position);
    }, 200);
}

// 改行したいけどできない
const encode = (str) => {
    // 改行を含む文字列の場合、改行コードをbrタグに変換する
    if (str.match("\n") !== null) {
        str = str.replace(/\n/g, "<br>")
    }
    return str
}

</script>

<template>
<!-- シミュレータ -->
<div class="simulator-main">
    <!-- カルーセル -->
    <carousel ref="myCarousel" v-model="currentSlide">
        <slide v-for="question in questionArray" :key="question.id">
            <div class="carousel__item">
                <h3 class="simulator-question">
                    <span>Q</span>{{ question.question }}
                </h3>
                <div class="simulator-answer-area">
                    <div v-for="answer in question.answerArray" :key="answer"
                    @click="btnClick({currentSlide}, questionArray, question.id, answer, $event.currentTarget); lastBntChecked(question.id) "
                    class="simulator-answerBtn">
                        <div v-if="answer.image"         class="answerBtn-Image"><img :src="answer.image"></div>
                        <div v-if="question.id === 3 && answer.answerId !== 4" class="answerBtn-mainText bold">{{ encode(answer.mainText) }}</div>
                        <div v-else-if="answer.mainText" class="answerBtn-mainText">{{ encode(answer.mainText) }}</div>
                        <div v-if="answer.subText"       class="answerBtn-subText">{{ answer.subText }}</div>
                        <div v-if="answer.optionPrice"   class="answerBtn-optionPrice">税込<span>{{ answer.optionPrice }}</span>円</div>
                        <div v-if="answer.campaignPrice" class="answerBtn-campaignPrice">税込<span>{{ answer.campaignPrice }}</span>円</div>
                        <div v-if="answer.bandMsg === 'セール'"    class="answerBtn-bandMsg_sale">{{ answer.bandMsg }}</div>
                        <div v-if="answer.bandMsg === 'オススメ!'" class="answerBtn-bandMsg_recommend"><div class="tri"></div>{{ answer.bandMsg }}</div>
                    </div>
                </div>
                <div class="simulator-answer-subarea">
                    <div v-if="question.desc"     class="simulator-question_desc">{{ question.desc }}</div>
                    <div v-if="question.subDesc"  class="simulator-question_subDesc">{{ question.subDesc }}</div>
                    <div v-if="question.descLink" class="simulator-question_descLink"><a href="#"><img :src="question.descLink"></a></div>
                </div>
            </div>
        </slide>
    </carousel>
    <p>
        <button class="yellowBtn" @click="carouselBack($event.currentTarget)" :class="{ disabled: !isBackBtnActive }" :disabled="!isBackBtnActive">
            <i class="fa fa-angle-left" aria-hidden="true"></i>前の設問にもどる
        </button>
    </p>
</div>
<!-- 診断結果 -->
<DiagnosticResult @init="initialize" :userAnswers="userAnswers" :lastBtnChecked="isLastBtnChecked" :autoScroll="autoScroll"></DiagnosticResult>

</template>

<style scoped>
.simulator-main {
    width: 100%;
    margin: 0 auto;
    text-align: center;
}
.simulator-question {
    font-size: 2.4rem;
    margin-bottom: 30px;
}
.simulator-question span {
    display: inline-block;
    background-color: #ff50ad;
    color: #fff;
    padding: 5px 12px;
    margin-right: 5px;
}
.simulator-answer-area {
    display: flex;
    justify-content: center;
    gap: 15px;
    margin-bottom: 10px;
}
.simulator-answerBtn {
    position: relative;
    display: block;
    min-height: 180px;
    width: 200px;
    border-radius: 8px;
    background-color: #f3f3f3;
    border: 2px solid #ccc;
    cursor: pointer;
}
.answerBtn-Image {
    margin-top: 10px;
    padding: 10px;
}
.answerBtn-Image img {
    height: 60px;
    width: 80px;
}
.answerBtn-mainText {
    font-size: 1.6rem;
}
.bold {
    font-size: 1.8rem;
    font-weight: bold;
}
.selectedMainText {
    font-weight: bold;
    color: #ff50ad;
}
.answerBtn-subText {
    font-size: 1.3rem;
}
.answerBtn-optionPrice {
    text-decoration: line-through;
    font-size: 1.1rem;
}

.answerBtn-campaignPrice {
    font-size: 1.4rem;
    font-weight: bold;
    color: #ff50ad;
    margin-bottom: 10px;
}
.answerBtn-campaignPrice span {
    font-size: 1.6rem;
}
.simulator-answer-subarea {
    position: relative;
    width: 50%;
    text-align: left;
    margin: 0 auto;
}
.simulator-question_desc {
    font-size: 1.2rem;
}
.simulator-question_subDesc {
    font-size: 1.2rem;
    color: red;
}
.simulator-question_descLink {
    position: absolute;
    right: 0;
    bottom: -40px;
}

.answered {
    background-color: #f265b07b;
    border: 2px solid #ff50ad;
}
.notAnswered {
    opacity: 0.6;
}
.answered-text {
    font-weight: bold;
    color: #ff50ad;
}

.fa-angle-left,
.fa-angle-down {
    font-size: 1.4em;
    margin-right: 15px;
}
.fa-times {
    margin-right: 5px;
}
.fa-plus {
    margin-right: 10px;
}

.carousel__item {
    width: 100%;
    height: 350px;
}
.answerBtn-bandMsg_sale {
    position: absolute;
    font-size: 1.4rem;
    background-color: red;
    top: 0;
    right: 15px;
    padding: 0 5px;
    color: #fff;
    font-weight: bold;
}
.answerBtn-bandMsg_recommend {
    position: absolute;
    font-size: 1.4rem;
    background-color: #ff50ad;
    top: 0;
    right: 0;
    padding: 0 5px;
    color: #fff;
    border-radius: 0 5px 0 3px;
    font-weight: bold;
    z-index: 10;
}

.tri {
    position: absolute;
    content: "";
    left: -14px;
    height: 0;
    width: 0;
    border-top: 0px solid transparent;
    border-right: 15px solid #ff50ad;
    border-bottom: 20px solid transparent;
    z-index: 0;
}

</style>

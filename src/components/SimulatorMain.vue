<script setup>
import DiagnosticResult from "./DiagnosticResult";
import RecommendArea from "./RecommendArea";

import "vue3-carousel/dist/carousel.css";
import { Carousel, Slide } from "vue3-carousel";
import { ref , reactive} from 'vue'

// カルーセル
const myCarousel = ref(null)
// 質問の内容と選択肢が格納されたデータ
const questionArray = [
    {
        id: 1,
        question: "電話番号（090等）は必要ですか？",
        answerArray: [
            {
                answerId: 1,
                image: require("../assets/img/1-1.png"),
                mainText: `必要`,
                subText:"（お乗り換えの方はこちら）",
            },
            {
                answerId: 2,
                image: require("../assets/img/1-2.png"),
                mainText: `不要`,
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
                mainText: `SIMカード`,
                subText: `（カード型）`,
            },
            {
                answerId: 2,
                image: require("../assets/img/2-1.png"),
                mainText: `eSIM*`,
            }
        ]
    },
    {
        id: 3,
        question: "通話定額を使いますか？",
        answerArray: [
            {
                answerId: 1,
                image: require("../assets/img/3-1.png"),
                mainText: `通話定額5分＋`,
                subText: `1回5分以内の国内通話無料`,
                smallText: `税込500円`,
                price: `税込90円`,
                bandMsg: "セール"
            },
            {
                answerId: 2,
                image: require("../assets/img/3-2.png"),
                mainText: `通話定額10分＋`,
                subText: `1回10分以内の国内通話無料`,
                smallText: `税込700円`,
                price: `税込290円`,
                bandMsg: "セール"
            },
            {
                answerId: 3,
                image: require("../assets/img/3-3.png"),
                mainText: `かけ放題＋`,
                subText: `無制限で国内通話無料`,
                smallText: `税込1400円`,
                price: `税込990円`,
                bandMsg: "セール"
            },
            {
                answerId: 4,
                image: require("../assets/img/3-4.png"),
                mainText: `通話定額は使わない`,
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
                mainText: `SMSを使う`,
            },
            {
                answerId: 2,
                image: require("../assets/img/4-2.png"),
                mainText: `SMSは使わない`,
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
                mainText: `データeSIMを使う`,
            },
            {
                answerId: 2,
                image: require("../assets/img/5-2.png"),
                mainText: `データeSMSは使わない`,
            },
        ]
    },
    {
        id: 6,
        question: "データ通信は毎月どのくらいご利用ですか？",
        answerArray: [
            {
                answerId: 1,
                image: require("../assets/img/6-1.png"),
                mainText: `家のWi-Fi利用が中心\n外出先でネットはあまり使わない`,
            },
            {
                answerId: 2,
                image: require("../assets/img/6-2.png"),
                mainText: `外出先のSNS利用やネット閲覧が中心`,
                bandMsg: "オススメ!"
            },
            {
                answerId: 1,
                image: require("../assets/img/6-3.png"),
                mainText: `外出先でゲームアプリを使う`,
            },
            {
                answerId: 4,
                image: require("../assets/img/6-4.png"),
                mainText: `外出先で動画を視聴しネットも良く使う`,
            },
            {
                answerId: 5,
                image: require("../assets/img/6-5.png"),
                mainText: `外出先でギガ（データ量）を気にせずに使いたい`,
            },
        ]
    },
]
// ユーザが選択した各選択肢を保存しておくためのデータ
const userAnswers = reactive([])

// 回答ボタンを押した際に走るメソッド
function btnClick(questionArray, currentQuestionId, answer, currentTarget) {
    // 選択したボタンの色を変更するために都度状態を見る必要がある
    const answerBtnAll = document.querySelectorAll(".simulator-answerBtn")
    const currentQuestions = []
    let btnIndex = 0;
    questionArray.forEach((question) => {
        if (question.id === currentQuestionId) {
            for (let i = 0; i < question.answerArray.length; i++) {
                currentQuestions.push(answerBtnAll[btnIndex + i])
            }
        } else {
            btnIndex += question.answerArray.length
        }
    })
    currentQuestions.forEach((elem) => {
        elem.classList.remove("answered")
        if (elem === currentTarget) {
            elem.classList.add("answered")
        }
    })

    const currentAnswer = {}
    let overwrite = false
    userAnswers.forEach((answered) => {
        // 今回選択した回答が回答済みの場合
        if (answered.questionId === currentQuestionId) {
            // 今回の回答に上書き
            answered.answerId = answer.answerId
            overwrite = true
        }
    })
    if (userAnswers.length < 1 || !overwrite) {
        // ユーザー回答が1つもされていない場合は情報を上書きしてプッシュ
        currentAnswer.questionId = currentQuestionId
        currentAnswer.answerId = answer.answerId
        userAnswers.push(currentAnswer)
    }
    console.log(userAnswers)
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
                    <button v-for="answer in question.answerArray" :key="answer"
                    @click="btnClick(questionArray, question.id, answer, $event.currentTarget); myCarousel.next()"
                    class="simulator-answerBtn">
                        <div v-if="answer.image" class="answerBtnImage"><img :src="answer.image"></div>
                        <div v-if="answer.mainText">{{ encode(answer.mainText) }}</div>
                        <div v-if="answer.subText">{{ answer.subText }}</div>
                        <div v-if="answer.smallText">{{ answer.smallText }}</div>
                        <div v-if="answer.price">{{ answer.price }}</div>
                        <div v-if="answer.bandMsg" class="bandMsg">{{ answer.bandMsg }}</div>
                    </button>
                </div>
            </div>
        </slide>
    </carousel>
    <p id="test"></p>
    <p>
        <button class="yellowBtn" @click="myCarousel.prev">
            <i class="fa fa-angle-left" aria-hidden="true"></i
            >前の設問にもどる
        </button>
    </p>
</div>
<!-- 診断結果 -->
<DiagnosticResult></DiagnosticResult>
<!-- おすすめプラン表示 -->
<RecommendArea></RecommendArea>
</template>



<style>
.simulator-main {
    width: 100%;
    margin: 0 auto;
    text-align: center;
}
.simulator-question {
    font-size: 2.0rem;
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
    align-items: center;
    gap: 20px;
    height: 200px;
}
.simulator-answerBtn {
    position: relative;
    display: block;
    height: 160px;
    width: 180px;
    border-radius: 8px;
    background-color: #fafafa;
    border: 2px solid #ccc;
}
.answered {
    font-weight: bold;
    color: #ff50ad;;
    background-color: #f265b07b;
    border: 2px solid #ff50ad;
}

.answerBtnImage {
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
    height: 400px;
}
.bandMsg {
    position: absolute;
    background-color: red;
    top: 0;
    right: 15px;
    padding: 0 5px;
    color: #fff;
    font-weight: bold;
}
</style>

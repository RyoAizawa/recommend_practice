<script setup>
import GlobalHeader from "./components/GlobalHeader";
import HeadLine from "./components/HeadLine";
import GlobalFooter from "./components/GlobalFooter";
import DiagnosticResult from './components/DiagnosticResult'
import RecommendArea from './components/RecommendArea'
</script>

<style>
/*--------------------
    初期化
--------------------*/
#app {
    font-family: Avenir, Helvetica, Arial, sans-serif;
    font-size: 1.6rem;
    color: #2c3e50;
    background-size: auto auto;
    background-color: rgba(232, 220, 193, 1);
    background-image: repeating-linear-gradient(
        134deg,
        transparent,
        transparent 7px,
        rgba(243, 248, 225, 0.72) 7px,
        rgba(229, 234, 211, 0.72) 14px
    );
}
button:active,
button:focus {
    outline: none;
}
button:hover {
    opacity: 0.7;
}
.container {
    width: 900px;
    margin: 30px auto;
}
small {
    font-size: 1.0rem;
}

/*--------------------
    料金診断領域
--------------------*/
.simulator-area {
    background-color: #fff;
    padding: 30px;
}
.simulator-main {
    width: 800px;
    margin: 0 auto;
    text-align: center;
}
.simulator-question span {
    background-color: #ff50ad;
    color: #fff;
    padding: 10px 12px;
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
    display: block;
    height: 150px;
    width: 170px;
    border-radius: 8px;
    background-color: #fafafa;
    border: 2px solid #ccc;
}
.yellowBtn {
    font-weight: bold;
    height: 45px;
    padding: 0 15px;
    background-color: #fff;
    color: #ab8a11;
    border: 2px solid #ab8a11;
    border-radius: 40px;
}
.yellowBlockNumber {
    width: 25px;
    height: 25px;
    color: #fff;
    background-color: #937813;
    text-align: center;
}

.fa-angle-left,
.fa-angle-down {
    font-size: 1.4em;
    margin-right: 15px;
}
.deleteBtn {
    color: #fff;
    border: none;
    background-color: #aaa;
    font-size: 1.4rem;
    border-radius: 20px;
}
.fa-times {
    margin-right: 5px;
}
.fa-plus {
    margin-right: 10px;
}


</style>

<template>
    <GlobalHeader />
    <main>
        <div class="container">
            <!-- ページ見出し -->
            <HeadLine></HeadLine>
            <div class="simulator-area">
                <!-- シミュレータ -->
                <div class="simulator-main">
                    <div class="slick-track">
                        <h3 class="simulator-question">
                            <span>Q</span>電話番号（090等）は必要ですか？
                        </h3>
                        <div class="simulator-answer-area">
                            <button class="simulator-answerBtn"></button>
                            <button class="simulator-answerBtn"></button>
                        </div>
                    </div>
                    <p>
                        <button class="yellowBtn">
                            <i class="fa fa-angle-left" aria-hidden="true"></i
                            >前の設問にもどる
                        </button>
                    </p>
                </div>
                <!-- 診断結果 -->
                <DiagnosticResult></DiagnosticResult>
                <!-- おすすめプラン表示 -->
                <RecommendArea></RecommendArea>
                <!-- 注釈 -->

            </div>
        </div>
    </main>
    <GlobalFooter />
</template>

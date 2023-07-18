<script setup>
import GlobalHeader from "./components/GlobalHeader";
import GlobalFooter from "./components/GlobalFooter";
import DiagnosticResult from './components/DiagnosticResult'
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
    ページ見出し
--------------------*/
.headline {
    text-align: center;
}
.headline > p:first-child {
    font-weight: bold;
}
.headline > h2 {
    font-size: 3.4rem;
    font-weight: normal;
    letter-spacing: 5px;
}
.circle {
    background-color: red;
    border-radius: 50%;
    padding: 5px 8px;
    margin: 0 3px;
    color: #fff;
}
.circle:nth-child(1) {
    background-color: #ec9f11;
}
.circle:nth-child(2) {
    background-color: #ee31cb;
}
.circle:nth-child(3) {
    background-color: #1796f0;
}
.circle:nth-child(4) {
    background-color: #0fc91f;
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
.notes-area {
    line-height: 1;
}




/*--------------------
    おすすめプラン領域
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
.recommend-item {
    background-color: #fff;
    width: 96%;
    border: 1px solid #aaa;
    border-radius: 5px;
    margin: 0 auto;
    padding: 8px;
}
.recommend-item-summery {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 15px;
    border-bottom: 1px solid #888;
}
.recommend-plan-icon {
    padding: 20px 10px 20px 20px;
}
.recommend-plan {
    display: flex;
    justify-content: space-between;
    width: 100%;
}
.recommend-plan>div {
    width: 25%;
}
.recommend-SIM {
    font-size: 2.6rem;
    font-weight: bold;
    color: #c929b1;
}
.recommend-GB {
    text-align: right;
    font-size: 2.6rem;
    font-weight: bold;
}
.recommend-price {
    text-align: right;
    font-size: 2.6rem;
    font-weight: bold;
}
.recommend-phone {
    text-align: right;
    font-size: 2.6rem;
    font-weight: bold;
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
.recommend-plan-price {
    border-top: 1px solid #888;
    border-bottom: 1px solid #888;
}
.recommend-plan-price>div {
    display: flex;
    justify-content: space-between;
    padding: 5px;
}
.recommend-plan-price>div~div {
    border-top: 1px dotted #888;
}
.recommend-detail {
    width: 45%;
    padding: 10px;
    font-size: 1.2rem;
    line-height: 1;
}
.recommend-detail p:first-child {
    font-size: 1.4rem;
    text-align: center;
    font-weight: bold;
    color: #ab8a11;
    padding: 10px;
}
.recommend-detail p:last-child {
    text-align: center;
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
.notes-area {
    font-size: 1.4rem;
}



</style>

<template>
    <GlobalHeader />
    <main>
        <div class="container">
            <div class="headline">
                <p>
                    <i class="fa fa-file-text-o" aria-hidden="true"></i
                    >あなたに最適なプランをご提案！
                </p>
                <h2> 料金プラン
                    <span class="circle">か</span>
                    <span class="circle">ん</span>
                    <span class="circle">た</span>
                    <span class="circle">ん</span>
                    診断
                </h2>
                <p>
                    最大4つの質問に答えるだけで、あなたにピッタリな料金プランをご提案！
                </p>
            </div>
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
                <div class="recommend">
                    <h3>あなたに最適なプランはこれ！</h3>
                    <button class="deleteBtn"><i class="fa fa-times" aria-hidden="true"></i>診断結果をすべて削除</button>
                    <div class="recommend-item">
                        <div class="recommend-item-summery">
                            <div class="yellowBlockNumber">1</div>
                            <div class="recommend-plan-icon">icon</div>
                            <div class="recommend-plan">
                                <div class="recommend-SIM">音声eSIM</div>
                                <div class="recommend-GB">15GB</div>
                                <div class="recommend-price">月額1,400円</div>
                                <div></div>
                            </div>
                        </div>
                        <div class="recommend-breakdownArea">
                            <div class="recommend-breakdown">
                                <p>内訳</p>
                                <div class="recommend-plan-price">
                                    <div>
                                        音声SIM5ギガプラン<div class="result-price">税込<span>990</span>円</div>
                                    </div>
                                    <div>
                                        通話定額10分＋<div class="result-price">税込<span>290</span>円</div>
                                    </div>
                                </div>
                            </div>
                            <div class="recommend-detail">
                                <p>こんな方にオススメ！</p>
                                <p><span class="point">&#9679;</span>データのみ利用で、できるだけお得に使いたい方</p>
                                <p><span class="point">&#9679;</span>ネット検索やゲームプレイをよく楽しむ方</p>
                                <p><img src="https://placekitten.com/200/150" alt="cat"></p>
                            </div>
                        </div>
                        <div class="recommend-text">
                            <p>このデータ量の利用目安（10GB）</p>
                            <p>・ホームページ閲覧：約34,100回・Twitter：約112,500回・IP電話：約290時間</p>
                            <small>※1 ホームページ閲覧：Yahoo!トップページ（モバイル版）を表示／※2 Twitter：公式アプリで全角140文字でツイート後リロードする／※3 IP電話：Skypeを使用し通話<br/></small>
                            <small>※弊社で独自に調査した結果であり、常にそのデータ量で利用できることを保証するものではありません。</small>
                        </div>
                    </div>
                    <button class="recommend-addBtn yellowBtn"><i class="fa fa-plus" aria-hidden="true"></i>2人目を診断する</button>
                    <button class="recommend-buyBtn"><i class="fa fa-chevron-right" aria-hidden="true"></i>ご購入・お申し込みはこちら</button>
                </div>
                <!-- 注釈 -->
                <div class="notes-area">
                    <small>※上記費用には、SIMカード発行手数料、ユニバーサルサービス料、電話リレーサービス料は含まれておりません。<br/></small>
                    <small>※消費税の端数処理により実際の請求額が異なる場合があります。<br/></small>
                    <small>※シミュレーション結果は目安です。ご契約条件やご請求額を保証するものではありませんので、あらかじめご了承の上ご利用ください。<br/></small>
                </div>
            </div>
        </div>
    </main>
    <GlobalFooter />
</template>

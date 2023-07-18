<script setup>
</script>

<style scope>

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
</style>

<template>
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
</template>

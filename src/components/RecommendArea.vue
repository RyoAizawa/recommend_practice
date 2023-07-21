<script setup>
const props = defineProps(["result"])

console.log(props)


</script>

<template>
<div class="recommend-item">
    <div class="recommend-item-summery">
        <div class="yellowBlockNumber">{{ props.result.id }}</div>
        <div class="recommend-plan-icon">icon</div>
        <div class="recommend-plan">
            <div class="recommend-SIM">{{ props.result.sim }}</div>
            <div class="recommend-GB">{{ props.result.dataVolume }}GB</div>
            <div class="recommend-price">月額{{ props.result.totalPrice.toLocaleString() }}円</div>
            <div v-if="(props.result.sim === '音声SIM' || props.result.sim === '音声eSIM' ) && props.result.option !== ''" class="recommend-option" >
                {{ props.result.option }}
            </div>
        </div>
    </div>
    <div class="recommend-breakdownArea">
        <div class="recommend-breakdown">
            <p>内訳</p>
            <div class="recommend-plan-price">
                <div>
                    {{ props.result.plan }}<div class="result-price">税込<span>{{ props.result.planPrice }}</span>円</div>
                </div>
                <div v-if="(props.result.sim === '音声SIM' || props.result.sim === '音声eSIM' ) && props.result.option !== ''">
                    {{ props.result.option }}<div class="result-price">税込<span>{{ props.result.optionPrice }}</span>円</div>
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
</template>



<style scoped>
/*--------------------
    おすすめプランアイテム
--------------------*/

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

.recommend-option {
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

</style>

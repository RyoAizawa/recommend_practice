<script setup>
</script>

<style scope>
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
    padding: 10px;
    margin: 10px;
    width: 25%;
}
.result-firstArea div:nth-of-type(2),
.result-firstArea div:nth-of-type(3) {
    border-left: 1px solid #333;
}
.breakdownBtn {
    position: absolute;
    top: 10px;
    right: 20px;
}

/* 内訳 */
.result-breakdownArea {
    padding: 15px;
}
.result-breakdownTable {
    display: flex;
/*    align-items: center;*/
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
.result-breakdown-item ~ .result-breakdown-item {
    border-top: 1px solid #ab8a11;
}
.result-plan {
    display: flex;
    background: #e8e4da;
    width: 100%;
}
.result-plan-numLabel {
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: normal;
    width: 10%;
}
.result-plan-price {
    width: 90%;
}
.result-plan-price > div {
    display: flex;
    justify-content: space-between;
    padding: 10px 5px;
    background-color: #fff;
}
.result-plan-price > div:first-child {
    border-bottom: 1px dotted #333;
}

.result-deleteBtnArea {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 20%;
    background-color: #fff;
}
</style>

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
                        <div>回線数<span>-</span>回線</div>
                        <div>データ容量<span>-</span>GB</div>
                        <div>月額<span>-</span>円</div>
                        <button class="yellowBtn breakdownBtn">
                            <i class="fa fa-angle-down" aria-hidden="true"></i>内訳
                        </button>
                    </div>
                    <!-- 内訳を押すと出力 -->
                    <div class="result-breakdownArea">
                        <div class="result-breakdownTable">
                            <div class="result-breakdown-label">
                                内訳
                            </div>
                            <div class="result-breakdown-items">
                                <div class="result-breakdown-item">
                                    <div class="result-plan">
                                        <div class="result-plan-numLabel">
                                            <span class="yellowBlockNumber">1</span>
                                        </div>
                                        <div class="result-plan-price">
                                            <div>
                                                音声SIM5ギガプラン<div class="result-price"><span>990</span>円</div>
                                            </div>
                                            <div>
                                                通話定額10分＋<div class="result-price"><span>290</span>円</div>
                                            </div>
                                        </div>
                                        <div class="result-deleteBtnArea">
                                            <button class="deleteBtn"><i class="fa fa-times" aria-hidden="true"></i>削除</button>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <small>※通話定額割引はご利用開始月より6ヵ月間適用</small>
            </div>
        </div>
    </div>
</template>

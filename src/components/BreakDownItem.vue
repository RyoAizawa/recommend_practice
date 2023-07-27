<script setup>
const props = defineProps(["result", "campaign"])
const emit = defineEmits(["deleteItem"])
const deleteItem = (target) => {
    const deleteBtnAll = document.querySelectorAll(".deleteBtn")
    let deleteBtnIndex = 0;
    deleteBtnAll.forEach((deleteBtn, index) => {
        console.log(index)
        if (deleteBtn === target) {
            deleteBtnIndex = index
        }
    })
    // resultArrayに保存済みの診断結果は現在シミュレート中の結果は含まないのでインデックスは-1する
    // 表示は逆順にしており、削除処理は大元の正順の配列に対し削除処理を行うため
    // 全削除ボタンの個数-逆順で取得したインデックス-現在処理中のボタンは除く（-lengthは1始まりなので-1で調整）処理を行う
    emit("deleteItem", (deleteBtnAll.length - deleteBtnIndex - 2))
}
</script>

<template>
    <div class="result-breakdown-item">
        <div class="result-plan">
            <div class="result-plan-numLabel">
                <span class="yellowBlockNumber">{{ props.result.id }}</span>
            </div>
            <div class="result-plan-price">
                <div>
                    {{ props.result.plan }}
                    <div class="result-price">
                    <span>{{ props.result.planPrice.toLocaleString() }}</span>円</div>
                </div>
                <div v-if="(props.result.sim === '音声SIM' || props.result.sim === '音声eSIM' ) && props.result.option !== ''" class="option" >
                    通話定額{{ props.result.option }}+
                    <div class="result-price" :class="{ campaignPrice : props.campaign }">
                    <span>{{ props.result.optionPrice.toLocaleString() }}</span>円</div>
                </div>
            </div>
            <div class="result-deleteBtnArea">
                <button @click="deleteItem($event.currentTarget)" class="deleteBtn"><i class="fa fa-times" aria-hidden="true"></i><span>削除</span></button>
            </div>
        </div>
    </div>
</template>

<style scoped>
/* 内訳 */
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
    font-size: 1.8rem;
}
.result-plan-price > div {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 10px 5px;
    background-color: #fff;
}
.result-price {
    font-size: 2.0rem;
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
.campaignPrice>span {
    font-size: 2.4rem;
}
.option {
    border-top: 1px dotted #333;
    font-weight: normal;
}
.result-deleteBtnArea {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 20%;
    background-color: #fff;
}
@media screen and (max-width:768px) {
    .result-plan-price {
        font-size: 1.6rem;
    }
    .deleteBtn>span {
        display: none;
    }
    .deleteBtn {
        padding: 6px 8px;
        border-radius: 8px;
        font-size: 2.0rem;
    }
    .campaignPrice>span {
        font-size: 1.8rem;
    }
    .result-price {
        font-size: 1.6rem;
    }

}

</style>

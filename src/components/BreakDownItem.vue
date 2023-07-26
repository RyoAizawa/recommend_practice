<script setup>
const props = defineProps(["result"])

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
    emit("deleteItem", deleteBtnIndex - 1)
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
                    <div class="result-price">
                    <span>{{ props.result.optionPrice.toLocaleString() }}</span>円</div>
                </div>
            </div>
            <div class="result-deleteBtnArea">
                <button @click="deleteItem($event.currentTarget)" class="deleteBtn"><i class="fa fa-times" aria-hidden="true"></i>削除</button>
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
}
.result-plan-price > div {
    display: flex;
    justify-content: space-between;
    padding: 10px 5px;
    background-color: #fff;
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
</style>

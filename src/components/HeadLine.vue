<script setup>
</script>

<style scope>
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
</style>

<template>
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
</template>

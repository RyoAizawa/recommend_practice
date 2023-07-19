<script setup>
import GlobalHeader from "./components/GlobalHeader";
import HeadLine from "./components/HeadLine";
import SimulatorMain from './components/SimulatorMain';
import GlobalFooter from "./components/GlobalFooter";
import AnnotationArea from './components/AnnotationArea';
</script>

<style>
/*--------------------
    グローバル
--------------------*/
#app {
    font-family: "Helvetica Neue", Arial, "Hiragino Kaku Gothic ProN",
    "Hiragino Sans", Meiryo, sans-serif;
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
small {
    font-size: 1rem;
}
.container {
    width: 900px;
    margin: 30px auto;
}
/*--------------------
    パーツ
--------------------*/
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
.deleteBtn {
    color: #fff;
    border: none;
    background-color: #aaa;
    font-size: 1.4rem;
    border-radius: 20px;
}
/*--------------------
    料金診断領域
--------------------*/
.simulator-area {
    background-color: #fff;
    padding: 30px;
}

</style>

<template>
    <GlobalHeader />
    <main>
        <div class="container">
            <!-- ページ見出し -->
            <HeadLine />
            <!-- シミュレータ表示領域 -->
            <div class="simulator-area">
                <!-- シミュレータ -->
                <SimulatorMain></SimulatorMain>
                <!-- 注釈 -->
                <AnnotationArea />
            </div>
        </div>
    </main>
    <GlobalFooter />
</template>

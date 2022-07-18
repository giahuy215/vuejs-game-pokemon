<template>
    <div class="card" :class="{ disabled : isDisabled}" @click="isFlipping" :style="{
        height: `${(720 - 16 * 4) / Math.sqrt(cardContext.length) - 16}px`,
        width: `${(((720 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3 ) / 4}px`,
        perspective: `${(((720 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3 ) / 4 * 2}px`,

    }">
        <div class="card-inner" :class="{ 'is-fliped': isFliped }" @click="onToggleFlipCard">
            <div class="card-face card-face-front">
                <div class="card-content" :style="{
                    backgroundSize: `${(((720 - 16 * 4) / Math.sqrt(cardContext.length) - 16) * 3 ) / 4 / 3}px`
                }"></div>
            </div>
            <div class="card-face card-face-back">
                <div class="card-content" :style="{ backgroundImage: `url(${require('@/assets/' + imgBackFaceUrl)})`}"></div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    props: {
        imgBackFaceUrl: {
            type: String,
            required: true,
        },
        card: {
            type: [String, Number, Array, Object],
            
        },
        cardContext: {
            type: Array,
            default: function(){
                return []
            },
        }
    },

    data() {
        return {
            isFliped: false,
            isDisabled: false,

        }
    },
    methods: {
        onToggleFlipCard() {
            if(this.isDisabled) return false;
            this.isFliped = !this.isFliped;
            if(this.isFliped) this.$emit("onFlip", this.card);
        },
        onFlipBackCard() {
            this.isFliped = false;
        },
        onEnableDisableMode() {
            this.isDisabled = true;
        },
    },
}
</script>

<Style lang="css" scoped>
.card {
    display: inline-block;
    margin-bottom: 1rem;
    margin-right: 1rem ;
    perspective: 100px;
}
.card-inner {
    width: 100;
    height: 100%;
    transition: transform 1s;
    transform-style: preserve-3d; 
    cursor: pointer;
    position: relative;
}
.card-inner.is-fliped {
    transform: rotateY(-180deg); /**Cho thẻ xoay tại chỗ 180 độ */
}
.card-face {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden; /** để 2 thằng là mặt trước và sau */
    overflow: hidden;
    border-radius: 1rem;
    padding: 1rem;
    box-shadow: 0 3px 10px 3px rgba(0 , 0, 0, 0.2);
}

.card-face-front .card-content {
    background: url('../assets/images/icon_back.png') no-repeat center center;
    height: 100%;
    width: 100%;
}

.card-face-back {
    background-color: var(--light);
    transform: rotateY(-180deg);
}

.card-face-back .card-content {
    background-size: contain;
    background-position: center center;
    background-repeat: no-repeat;
    height: 100%;
    width: 100%;
}
.card.disabled .card-inner{
    cursor: default;
}


</Style>
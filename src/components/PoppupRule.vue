<template>
  <div class="wrapper-popup" v-bind:class="getClasPopup">
    <div class="rule">
        <h3>Luật chơi</h3>
        <p>
            Cách 1: Chơi riêng/ Single Hand
            Mỗi người chơi có một bộ năm viên xúc xắc 6 mặt tiêu chuẩn.  Mỗi người chơi tung một lần và đưa giá cược liên quan đến xúc xắc mà mỗi người chơi có thể nhìn thấy (bàn tay của họ) cộng với tất cả các viên xúc xắc được giấu (tay của người chơi khác).
            Người chiến thắng trò chơi là người chơi cuối cùng còn lại bất kỳ viên xúc xắc nào.
            <br>
            Cách 2: Chơi chung/ Common Hand
            Tất cả người chơi cùng chia sẻ năm viên xúc xắc, chúng được chuyển xung quanh bàn.  Giá cược liên quan đến viên xúc xắc khi chúng ở trước mặt người trả giá,  sau khi viên xúc xắc được chọn được tung lại. Người chiến thắng trò chơi là người chơi cuối cùng còn lại bất kỳ quầy nào.
        </p>
        <button class="confirm" v-on:click="confirm">Đã hiểu</button>
    </div>
  </div>
</template>

<script>
export default {
    name:'popup-rule',
    props:{
        isOpenPopup:Boolean
    },
    data(){
        return{

        }
    },
    methods:{
        confirm(){
            this.$emit('handleConfirm')
        }
    },
    computed:{
        getClasPopup: function(){
            return{
                'open-popup':this.isOpenPopup
            }
        }
    }
}
</script>

<style>
.wrapper-popup{
    position: fixed;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(0,0,0,0,4);
    opacity: 0;
    visibility: hidden;
}
.wrapper-popup.open-popup{
    opacity: 1;
    visibility: visible;
}
.rule{
    width: 500px;
    top: 50%;
    left: 50%;
    background: #fff;
    transform: translate(-50%,-50%) scale(1.2);
    justify-content: center;
    position: absolute;   
    padding: 20px;
    font-family: Arial, Helvetica, sans-serif;
}
.open-popup .rule{
    transform: translate(-50%,-50%) scale(1);
}
.confirm{
    padding: 8px 15px;
    cursor: pointer;
    margin-top: 20px;
    background: #fff;
}
.confirm:hover{
    color: #fff;
    background: #333;
}
</style>
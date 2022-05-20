<template>
  <div class="wrapper-popup" :class="getClassPopup">
      <div class="rule">
        <h3>Luật chơi</h3>
        <p>Chơi đê bạn</p>
        <button 
            @click="confirm"
            class="confirm"
        >Đã hiểu</button>
      </div>        
  </div>
</template>

<script>
export default {
    name:'popup-rule',
    data(){
        return{

        }
    },
    props:{
        isOpenPopup: { type: Boolean, default: false } 
    },
    methods:{
        confirm(){
            this.$emit('handleConfirm')
        }
    },
    computed: {
        getClassPopup:function() {
            return{
                'open-popup': this.isOpenPopup,
            } 
        }
    }
}
</script>

<style>
    .wrapper-popup {
        position: fixed;
        top: 0px;
        left: 0px;
        right: 0px;
        bottom: 0px;
        background-color: rgba(0, 0, 0, 0.4);
        opacity: 0;
        visibility: hidden;
        transition: all .3s ease;
    }
    
    .wrapper-popup.open-popup {
        opacity: 1;
        visibility: visible;
    }

    .open-popup .rule{
        transform: translate(-50%, -50%), scale(1);
    }

    .rule{
        font-family: Arial, Helvetica, sans-serif;
        width: 300px;
        padding: 20px;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%), scale(1.2);
        background-color: #fff;
        position: absolute;
    }
    
    .rule h3{
        margin-bottom: 10px;
    }

    .rule .confirm{
        padding: 8px 15px;
        margin-top: 20px;
        cursor: pointer;
        background-color: #fff;
        border: 3px solid #333;
        transition: all .3s ease;
    }

    .rule .confirm:hover{
        background-color: #333;
        color: #fff;
    }
</style>
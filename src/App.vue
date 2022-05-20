<template>
	<div id="app">
		<div class="wrapper clearfix">
            <players
				:scorePlayer="scorePlayer"
				:activePlayer="activePlayer"
				:currentScore="currentScore"
				:isWinner="isWinner"
			/>            
            <controls
				:isPlaying="isPlaying" 
				@handleNewGame="handleNewGame"
				@handleRollDices="handleRollDices"
				@handleHoldScore="handleHoldScore"
				:finalScore="finalScore"
				@handleChangeFinalScore="handleChangeFinalScore"
			/>
            <dice-balls
				:diceBalls="diceBalls" 
			/>
			<popup-rule
				@handleConfirm="handleConfirm" 
				:isOpenPopup="isOpenPopup"
			/>           
        </div>
	</div>
</template>

<script>
import Controls from './components/Controls.vue'
import DiceBalls from './components/DiceBalls.vue'
import Players from './components/Players.vue'
import PopupRule from './components/PopupRule.vue'

export default {
	name: 'app',
	data () {
		return {
			isPlaying: false, // Check đang chơi game không?
			isOpenPopup: false,
			activePlayer: 0, //Ai đang là người chơi hiện tại
			scorePlayer: [1,2],
			diceBalls:[3, 3],
			currentScore: 0,
			finalScore: 100
		}
	},
	components: {
		Players,
		Controls,
		DiceBalls,
		PopupRule,
	},

	computed:{
		isWinner(){
			let { scorePlayer, finalScore } = this;

			if( scorePlayer[0] >= finalScore || scorePlayer[1] >= finalScore ){
				// Dừng cuộc chơi
				this.isPlaying = false;
				return true;
			}
			return false;
		}
	},

	methods: {
		nextPlayer(){
			this.activePlayer = this.activePlayer === 0 ? 1 : 0;
			this.currentScore = 0;
		},

		handleConfirm(){
			this.isPlaying = true,
			this.isOpenPopup = false,
			this.scorePlayer = [0, 0],
			this.activePlayer = 0,
			this.currentScore = 0,
			this.diceBalls = [6,6] 
		},

		handleNewGame(){
			console.log('New Game'),
			this.isOpenPopup = true
		},

		handleRollDices(){
			if(this.isPlaying) {
				//Xoay xúc xắc
				//Math.random() chỉ random từ 0 tới 1
				var dice1 = Math.floor(Math.random() * 6) + 1;
				var dice2 = Math.floor(Math.random() * 6) + 1;
				this.diceBalls = [dice1, dice2]

				if(dice1 == 1 || dice2 == 1) {
					let activePlayer = this.activePlayer;
					setTimeout(function(){
						alert(`Bạn ${activePlayer + 1} quá đen roll vào 1`);
					},10);
					this.nextPlayer();
				}else {
					this.currentScore = this.currentScore + dice1 + dice2;
				}
			} else {
				alert('Vui lòng New Game')
			}
		},

		handleHoldScore(){
			if (this.isPlaying){
				let {scorePlayer, activePlayer, currentScore} = this; 			//  Destructuring ES6
				let scoreOld = scorePlayer[activePlayer];
				// let cloneScorePlayer = [...scorePlayer];						// Tạo biến với địa chỉ mới để copy dữ liệu tránh lỗi của Destructuring
				// 	cloneScorePlayer[activePlayer] = scoreOld + currentScore;
					
				// this.scorePlayer = cloneScorePlayer;							// Xử lý xong dữ liệu trả về cho đối tượng ở địa chỉ ban đầu

				this.$set(this.scorePlayer, activePlayer, scoreOld + currentScore);

				if(!this.isWinner) {
					this.nextPlayer();
				}
			} else{
				alert('Vui lòng New Game');
			}
		},

		handleChangeFinalScore(e) {
			var number = parseInt(e.target.value)
			if(isNaN(number)){
				this.finalScore = '';
			} else {
				this.finalScore = number;
			}
		}
	}
}
</script>
	<style>
		* {
		margin: 0;
		padding: 0;
		box-sizing: border-box;		
	}

	.clearfix::after {
		content: "";
		display: table;
		clear: both;
	}

	body {
		background-image: linear-gradient(rgba(62, 20, 20, 0.4), rgba(62, 20, 20, 0.4)), url(/public/assets/back.jpg);
		background-size: cover;
		background-position: center;
		font-family: Lato;
		font-weight: 300;
		position: relative;
		height: 100vh;
		color: #555;
	}

	.wrapper {
		width: 1000px;
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		background-color: #fff;
		box-shadow: 0px 10px 50px rgba(0, 0, 0, 0.3);
		overflow: hidden;
	}
</style>

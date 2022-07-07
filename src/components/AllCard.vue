<template>
	<div class="card" :class="flippedStyles" @click="selectCard">
		<div class="card-face is-front">
			<img :src="`/image/${value}.png`" :alt="value"/>
			<img v-if="matched" class="icon-checkmark" src ="/image/checkmark.svg" />
		</div>
		<div class="card-face is-back"></div>
	</div>
</template>


<script>
import { computed } from 'vue'

export default {
	props:{
		position:{
			type: Number,
			required: true
		},
		value: {
			type: String,
			required: true
		},
		visible: {
			type: Boolean,
			default: false
		},
		matched:{
			type: Boolean,
			default: false
		}
	},

	setup(props, context){

		const flippedStyles = computed(() => {
      if (props.visible) {
        return 'is-flipped'
      }
    })

		const selectCard = () => {
			context.emit('select-card',{
				position: props.position,
				faceValue:props.value,
			})
		}

		return {
			flippedStyles,
			selectCard
		}
	}
}
</script>

<style>
.card{
  position: relative;
  transition: 0.5s transform ease-in;
  transform-style: preserve-3d;
}
.card-face{
	width: 100%;
	height: 100%;
	position: absolute;
	border-radius: 10px;
	display: flex;
	align-items: center;
	justify-content: center;
	backface-visibility: hidden;
}

.card.is-flipped{
	transform: rotateY(180deg);
}

.card-face.is-front{
	background-image: url('../../public/image/card-bg.png');
	color: white;
	transform: rotateY(180deg);
}
.card-face.is-back{
	background-image: url('../../public/image/card-bg-empty.png');
	color: white;
}

.icon-checkmark{
	position: absolute;
	right: 5px;
	bottom: 5px;
}


</style>

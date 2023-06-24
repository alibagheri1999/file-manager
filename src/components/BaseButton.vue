<template>
  <button type="button"
          ref="button"
          :disabled="!isActive"
          :style="continuousStyleProperties"
          :class="{...predefinedDiscreteStyleProperties, ...{['button--shake-animation']: shakeAnimation}}"
          @mouseover="changeBackGround('over')"  @mouseleave="changeBackGround('leave')"
          @animationend="shakeAnimationEnded"
          @click.prevent="clickBtn"
  >
    {{ buttonInnerTxt }}
    <div class="button-slot" v-if="$slots['button-slot']">
      <slot name="button-slot"></slot>
    </div>
  </button>
</template>

<script>
export default {
  name : "BaseButton",
  data(){
    return {
      discreteStyleTypes : [
        'button--right-aligned',
        'button--left-aligned',
        'button--small',
        'button--big',
        'button--full',
        'button--semi-full',
        'button--curved',
        'button--disabled',
        'button--with-border'
      ],
    }
  },
  props: ['styleTypes', 'buttonInnerTxt', 'continuousParams', 'shakeAnimation','isActive'],
  emits : ['btn-clicked', 'shake-animation-ended'],
  computed: {
    predefinedDiscreteStyleProperties() {
      const styleTypes = {};
      if(this.styleTypes && this.styleTypes.some(type => this.discreteStyleTypes.includes(type)))
        this.styleTypes.forEach(type => {
          styleTypes[type] = true;
        });
      return styleTypes;
    },
    continuousStyleProperties() {
      const continuousParams = {};
      if(this.continuousParams){
        this.continuousParams.forEach(type => {
          if (type.includes('=')) {
            continuousParams[`${type.split('=')[0].trim()}`] = type.split('=')[1].trim().toString();
          }
        });
      }
      return continuousParams;
    }
  },
  methods : {
    clickBtn(){
      this.$emit('btn-clicked');
    },
    shakeAnimationEnded(){
      this.$emit('shake-animation-ended');
    },
    changeBackGround(type){
      if (this.$refs.button){
        if(type === 'over'){
          this.$refs.button.style.backgroundColor = '#4cc9b7'
        }else {
          this.$refs.button.style.backgroundColor = '#95d1cc'
        }


      }
    }
  }
}
</script>

<style scoped>

button {
  position: relative;
  font-size: 1.5rem;
  padding: 1rem 2.5rem;
  border: none;
  display: flex;
  flex-direction: row-reverse;
  justify-content: center;
  align-items: center;
  cursor: pointer;
  border-radius: 0.4rem;
}

.button-slot {
  margin-right: 1rem;
  display: flex;
  justify-content: center;
  align-items: center;
}


/* Button Types Classified with Its Direction Aspect */
.button--right-aligned{
  justify-content: flex-end;
}
.button--left-aligned{
  justify-content: flex-start;
}


/* Button Types Classified with Its Size Aspect */
.button--small {
  padding: .5rem 1.25rem;
  font-size: 1.5rem;
}
.login--button{
  padding: .85rem 11.2rem;
}
.login--button:hover{
  background-color: #4fd5ca !important;
}
.button--big{
  padding: .85rem 6.9rem;
}
.button--full{
  width: 100%;
}
.button--semi-full{
  width: 90%;
}


/* Button Types Classified with Its Visual Aspect */
.button--curved {
  border-radius: 2.5rem;
}

.button--disabled {
  padding: .85rem 2.5rem;
  background-color: #aaa !important;
  cursor: not-allowed;
}
.button--disabled :hover {
  background-color: #aaa !important;

}
.button--with-border{
  border : .2rem solid black
}


/* Button animation while have invalid data */
.button--shake-animation{
  animation: btn-shake 0.82s cubic-bezier(0.37, 0.08, 0.2, 0.98);
  transform: translate3d(0, 0, 0);
  backface-visibility: hidden;
  perspective: 100rem;
}
@keyframes btn-shake {
  10%,
  90% {
    transform: translate3d(-0.1rem, 0, 0);
  }

  20%,
  80% {
    transform: translate3d(0.2rem, 0, 0);
  }

  30%,
  50%,
  70% {
    transform: translate3d(-0.4rem, 0, 0);
  }

  40%,
  60% {
    transform: translate3d(0.4rem, 0, 0);
  }
}

@media only screen and (min-width: 250px) and (max-width: 1000px){
  .button--big{
    padding: .85rem 6.9rem;
  }
}
@media only screen and (max-width: 500px) {
  .button--big {
    padding: .5rem 3.9rem;

  }
}
</style>
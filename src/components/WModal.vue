<script setup lang="ts">

export interface ModalOptions {
  element: HTMLElement;
  content: string;
  index: number;
  stepsCount: number,
  isVisible: boolean,
  prevText?: string,
  nextText?: string,
  finishText?: string,
}

withDefaults(defineProps<ModalOptions>(), {
  nextText: 'Next',
  prevText: 'Previous',
  finishText: 'Finish',
})

const emits = defineEmits<{
  (e: 'prev'): void;
  (e: 'next'): void;
  (e: 'close'): void;
}>();

const prevStep = () => emits('prev');
const nextStep = () => emits('next');
const closeModal = () => emits('close')
const finishStep = () => {
  emits('next')
  emits('close')
}
</script>

<template>
    <div v-if="isVisible" class="walkthrough-modal">
      <div class="walkthrough-content">
        <div class="walkthrough-close-container">
          <button @click="closeModal" id="walkthrough-close" aria-label="Close button">X</button>
        </div>
        <div class="walkthrough-container">
          <div v-html="content"></div>
        </div>
        <div class="walkthrough-buttons">
          <div id="walkthrough-steps">
          <span
              v-for="n in stepsCount"
              :key="n"
              :class="{'current': n === index}"
              class="walkthrough-step">•</span>
          </div>
          <div>
            <button @click="prevStep" :disabled="index === 1" :aria-label="prevText">
              {{ prevText }}
            </button>
            <button @click="nextStep" v-if="index < stepsCount" :aria-label="nextText">
              {{ nextText }}
            </button>
            <button @click="finishStep" v-else :aria-label="finishText">
              {{ finishText }}
            </button>
          </div>
        </div>
        <div class="arrow arrow-top"></div>
        <div class="arrow inner inner-top"></div>
      </div>
    </div>
</template>

<style lang="scss">
.walkthrough-container {
  video, iframe, img {
    height: 200px;
    border-radius: 15px;
    aspect-ratio: 1;
  }
  video, iframe {
    max-width: 100%;
    aspect-ratio: 16 / 9;
  }
  img {
    aspect-ratio: 1 / 1;
  }
}
.walkthrough-highlight {
  outline: 2px solid #78290F;
  border-radius: 2px;
}
</style>
<style scoped lang="scss">
.walkthrough-modal {
  position: absolute;
  top: 0;
  left: 0;
  width: 100vw;
  display: flex;
  justify-content: start;
  align-items: center;
  z-index: 1001;

}

.walkthrough-content {
  position: static;
  background-color: #fff;
  box-shadow: rgba(50, 50, 93, 0.25) 0 13px 27px -5px, rgba(0, 0, 0, 0.3) 0 8px 16px -8px;
  padding: 20px 20px 10px;
  border: 2px solid #15616d;
  border-radius: 10px;
  text-align: center;
  max-width: 40%;
  margin: 10px;
  overflow: hidden;
  @media (max-width: 768px) {
    max-width: 70%;
    padding: 1em;
    max-height: 70vh;
  }
  @media (max-width: 480px) {
    max-width: 75%;
    padding: 0.8em;
    max-height: 60vh;
  }
  .walkthrough-close-container {
    position: relative;
    #walkthrough-close {
      position: absolute;
      right: 0;
      top: 0;
      border: none;
      background-color: #78290f;
      border-radius: 0;
      color: #fff;
      padding: 8px 11px;
      z-index: 1002;

      &:hover {
        background-color: rgba(120, 41, 15, 0.77);
      }
    }
  }
}


.walkthrough-buttons {
  margin-top: 20px;
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  place-items: center;
  gap: 15px;

  #walkthrough-steps {
    display: flex;
    flex-direction: row;
    gap: 5px;
    .walkthrough-step {
      color: rgba(10, 93, 101, 0.66);
    }
    .current {
      color: #15616d;
      font-weight: bolder;
    }
  }

  button {
    margin: 0 5px;
    border: none;
    background-color: #15616d;
    color: #fff;
    padding: 10px;
    border-radius: 5px;
    &:hover {
      background-color: rgba(10, 93, 101, 0.66);
    }
    &:disabled {
      background-color: rgba(10, 93, 101, 0.66);
      pointer-events: none;
    }
  }
}


.hidden {
  display: none;
}
.arrow {
  width: 0;
  height: 0;
  border: 10px solid transparent;
  position: absolute;
  left: 30px;
}

.arrow-top {
  border-bottom: 20px solid #15616d;
  top: -20px;
}

.arrow.inner-top {
  border-bottom: 20px solid #15616d;
  border-bottom-color: white;
  top: -13px;
  z-index: 1002;
}

.arrow-bottom {
  border-top: 20px solid #15616d;
  bottom: -20px;
}
.arrow.inner-bottom {
  border-top: 20px solid #15616d;
  border-top-color: white;
  bottom: -13px;
  z-index: 1002;
}
</style>
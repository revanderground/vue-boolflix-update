<template>
  <header>
    <div class="container-fluid ">
      <div class="row">
        <div class="col-4 d-flex align-items-center mt-1 ps-4">
          <a href="https://www.youtube.com/watch?v=li2WDMgHxD0"><img src="/logo.png" alt="logo netflix"></a>
        </div>
        <div class="col-7 d-flex align-items-center justify-content-end pe-4">
          <Transition>
            <span @click="changeStatusClicked()" v-if="(isClicked == false)" class="text-white"><i class="fa-solid fa-magnifying-glass"></i></span>
          </Transition>
          <Transition>
            <input type="text" placeholder="Cerca un Film, Serie TV" v-model="filmSearched" class="my-input-txt p-2 grow"
              @keyup.enter="sendFilmSearched()" v-if="isClickedInput" v-on:blur="changeStatusClickedInput()" autofocus>
          </Transition>
        </div>
        <div class="col-1 d-flex align-items-center justify-content-end">
          <div class="row">
              <div class="col-6">Bambini</div>
              <div class="col-2"><i class="fa-solid fa-bell"></i></div>
              <div class="col-4"><img src="https://i0.wp.com/ih0.redbubble.net/image.618379802.1473/flat,1000x1000,075,f.u2.jpg" alt="profile img" class="profile-pic"></div>
          </div>
        </div>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  props: {
  },
  data: function () {
    return {
      filmSearched: '',
      isClicked: false,
      isClickedInput: false,
    }
  },
  methods: {
    //! this method emit the user search to app.vue and after the research the input type text became ''
    sendFilmSearched() {
      this.$emit('filmSearched', this.filmSearched)
      this.filmSearched = ''
    },
    
    changeStatusClicked() {
      this.filmSearched = '',
        this.isClicked = !this.isClicked
      setTimeout(() => { this.changeStatusClickedInput() }, 600)
    },
    changeStatusClickedInput() {
      this.isClickedInput = !this.isClickedInput
      if (this.isClickedInput == false) {

        setTimeout(() => { this.isClicked = false }, 600)
      }
    },
  },
}
</script>

<style scoped lang="scss">
@import '../assets/style/variable.scss';

header {
  background-color: $bgColorMain;
  height: 100px;

  .profile-pic{
    width: 25px;
    height: 25px;
  }
  span {
    cursor: pointer;
  }

  .my-input-txt {
    background-color: $bgColorMain;
    color: white;
    border: solid 1px white;
  }

  img {
    width: 150px;
  }

  .v-enter-active,
  .v-leave-active {
    transition: opacity 0.2s ease-in;
  }

  .v-enter-from,
  .v-leave-to {
    opacity: 0;
  }

  .grow {
    animation: grow 0.3s ease-in;
  }

  @keyframes grow {
    from {
      width: 0;
      opacity: 0;
    }
    to {
      width: 16%;
      opacity: 1;	
    }
  }

}
</style>

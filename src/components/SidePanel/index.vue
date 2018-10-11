<template>
  <transition name="rightPanel-animate">

    <div
      v-show="value"
      ref="rightPanel"
      class="vs-content-sidebar">
      <div class="rightPanel-background"/>
      <div
        :class="[`vs-sidebar-${color}`]"
        class="vs-sidebar">
        <div class="vs-sidebar-items">
          <slot/>
        </div>
      </div>
    </div>
    <!-- <el-button type="primary" icon="el-icon-setting" circle/> -->

  </transition>
</template>

<script>
export default {
  name: 'RightPanel',
  props: {
    value: {
      default: false,
      type: Boolean
    },
    color: {
      default: 'primary',
      type: String
    },
    clickNotClose: {
      default: false,
      type: Boolean
    }
  },
  watch: {
    value() {
      if (this.value && !this.clickNotClose) {
        this.addEventClick()
      }
    }
  },
  mounted() {
    this.insertBody()
  },
  methods: {
    addEventClick() {
      window.addEventListener('click', this.closeSidebar)
    },
    closeSidebar(evt) {
      const parent = evt.target.closest('.vs-sidebar')
      if (!parent) {
        this.$emit('input', false)
        window.removeEventListener('click', this.closeSidebar)
      }
    },
    insertBody() {
      const elx = this.$refs.rightPanel
      const body = document.querySelector('body')
      body.insertBefore(elx, body.firstChild)
    }
  }
}
</script>

<style rel="stylesheet/scss" lang="scss" >
.rightPanel-background {
  background: rgba(0, 0, 0, .2);
  width: 100%;
  height: 100%;
  position: fixed;
  z-index: 20000;
  transition: all .3s ease;
  opacity: 1;
}

.vs-sidebar{
  background: rgb(255,255,255);
  z-index :3000;
  position: fixed;
  height: 100vh;
  width: 100%;
  max-width: 260px;
  top: 0px;
  display: flex;
  flex-direction: column;
  box-shadow: 0px 0px 15px 0px rgba(0,0,0,.05);
  left: 0px;
  transition: all .25s ease;
  z-index: 40000;
 left: auto;
    right: 0px;

}

// animations
.rightPanel-animate-enter-active,
.rightPanel-animate-leave-active {
  transition: all .25s ease;
  .vs-sidebar {
    transition: all .25s ease;

  }
}

.rightPanel-animate-enter,
.rightPanel-animate-leave-to {
  .vs-sidebar-background {
    opacity: 0 !important;
  }
  .vs-sidebar {
   transform: translate(100%);
  }
}

</style>

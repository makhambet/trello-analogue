<template>
  <div
    :style="{height: height + 'px' || 'auto'}"
    ref="task"
    class="task"
    :class="{'drag-active': onDrag}"
  >
    <div
      class="task__block"
      @mousedown="onMouseDown"
      @mouseup="onMouseUp()"
      @mousemove="onMouseMove"
      :class="{'drag-active': onDrag, 'move-active': onMove}"
      :style="{left: left ? `${left}px` : 'auto', top: top ? `${top}px` : 'auto'}"
    >
      <div
        ref="taskDetail"
        :contenteditable="contenteditable"
        @click="editTable()"
        class="task__details"
      >
        {{task}}
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    task: {
      type: String,
      default: ''
    },
    index: {
      type: Number,
      default: 0
    }
  },
  data() {
    return {
      onDrag: false,
      left: null,
      top: null,
      onMove: false,
      taskDetailHtml: null,
      taskHtml: null,
      height: null,
      horizontal: null,
      vertical: null,
      contenteditable: false,
      topBoundingClientRect: null
    }
  },
  computed: {
    positionChange() {
      return (this.top || this.topBoundingClientRect) - this.topBoundingClientRect
    }
  },
  methods: {
    onMouseDown(e) {
      if(!this.height) {
        this.height = this.taskDetailHtml.offsetHeight
      }
      this.topBoundingClientRect = this.taskDetailHtml.getBoundingClientRect().top
      this.vertical = e.clientY - this.topBoundingClientRect
      this.horizontal = e.clientX - this.taskDetailHtml.getBoundingClientRect().left
      this.onDrag = true
    },
    onMouseUp() {
      this.top = this.left = this.topBoundingClientRect = this.vertical = this.horizontal = null
      this.onDrag = false
      this.onMove = false
    },
    onMouseMove(e) {
      if(this.onDrag) {
        this.contenteditable = false
        this.onMove = true
        this.moveAt(e.clientX, e.clientY)
        this.$emit('changeCardList', this.index, this.positionChange)
      }
    },
    moveAt(clientX, clientY) {
      this.top = clientY - this.vertical
      this.left = clientX - this.horizontal
      // console.log(this.top, clientY, this.topBoundingClientRect, this.left, clientX, this.leftBoundingClientRect)
    },
    editTable() {
      // this.contenteditable = true
    }
  },
  mounted() {
    this.taskDetailHtml = this.$refs.taskDetail
  }
}
</script>

<style scoped>
  .task {
    /*position: relative;*/
    margin-bottom: 8px;
    cursor: pointer;
    box-sizing: border-box;
  }
  .task.drag-active {
    background: #c4c4c4;
    position: static;
    user-select: none;
  }
  .task__details {
    width: 100%;
    background: #FFF;
    padding: 6px 8px 2px;
    box-sizing: border-box;
    border-radius: 3px;
  }
  .task__block.drag-active {
    position: absolute;
    width: 272px;
    z-index: 9;
    user-select: none;
    padding: 200px;
    margin-left: -200px;
    margin-top: -200px;
  }
  .task__details.move-active {
    transform: rotate(-1deg);
  }
  .task__block.drag-active .task__details {
    box-shadow: 0 0 5px  #BDBDBD;
  }
</style>

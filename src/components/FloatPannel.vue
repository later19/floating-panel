<template>
  <div
    class="floating-panel"
    :style="{ height: panelHeight + 'px', bottom: '0' }"
    @touchstart="onTouchStart"
    @touchmove="onTouchMove"
    @touchend="onTouchEnd"
  >
    <div class="panel-header" @click="togglePanel">
      <span class="panel-icon"></span>
    </div>
    <div class="panel-content">
      <slot></slot>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      panelHeight: 100, // 初始面板高度（底部）
      startY: 0, // 触摸起始Y坐标
      currentY: 0, // 当前触摸Y坐标
      topPadding: 50, // 全屏状态下距离顶部的间距
      anchors: [100, 400, window.innerHeight - 53], // 锚点位置：底部、中间、“全屏”状态（带顶部间距）
      currentAnchorIndex: 0, // 当前锚点索引
    }
  },
  methods: {
    disableScroll() {
      document.body.style.overflow = 'hidden'
    },
    enableScroll() {
      document.body.style.overflow = ''
    },
    onTouchStart(event) {
      this.startY = event.touches[0].clientY
      this.currentY = this.panelHeight
      this.disableScroll() // 禁用页面滚动
    },
    onTouchMove(event) {
      const deltaY = event.touches[0].clientY - this.startY
      const newHeight = this.currentY - deltaY

      // 限制拖动的最小和最大高度
      if (newHeight >= this.anchors[0] && newHeight <= this.anchors[2]) {
        this.panelHeight = newHeight
      }
    },
    onTouchEnd() {
      this.enableScroll() // 恢复页面滚动

      // 找到最接近的锚点并设置当前高度
      let closestAnchorIndex = 0
      let minDistance = Math.abs(this.panelHeight - this.anchors[0])

      this.anchors.forEach((anchor, index) => {
        const distance = Math.abs(this.panelHeight - anchor)
        if (distance < minDistance) {
          minDistance = distance
          closestAnchorIndex = index
        }
      })

      this.panelHeight = this.anchors[closestAnchorIndex]
      this.currentAnchorIndex = closestAnchorIndex
    },
    togglePanel() {
      // 在锚点间循环切换：底部 -> 中间 -> 全屏
      this.currentAnchorIndex =
        (this.currentAnchorIndex + 1) % this.anchors.length
      this.panelHeight = this.anchors[this.currentAnchorIndex]
    },
  },
}
</script>

<style scoped>
.floating-panel {
  position: fixed;
  bottom: 0;
  left: 0;
  right: 0;
  background-color: #fff;
  border-top-left-radius: 10px;
  border-top-right-radius: 10px;
  box-shadow: 0 -2px 8px rgba(0, 0, 0, 0.15);
  transition: height 0.3s ease;
  overflow: hidden;
}
.panel-header {
  padding: 6px;
  background-color: #fff;
  cursor: pointer;
  display: flex;
  justify-content: center;
}
.panel-icon {
  display: inline-block;
  width: 40px;
  height: 3px;
  border-radius: 3px;
  background: #b3b2b2;
}
.panel-content {
  padding: 16px;
}
</style>

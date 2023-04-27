<template>
  <div class="container" id="container">
    <!-- 可以在这里放置 DOM 元素 -->
    <slot></slot>
    <div class="drag-area" id="dragArea"></div>
  </div>
</template>

<script>
export default {
  name: 'dragArea',
  props: {
    //设置被选中dom的class,默认为selected
    selectClass: {
      required: false,
      default: 'selected',
    },
  },
  mounted: function () {
    const selectClass = this.selectClass;
    const container = document.getElementById('container');
    const dragArea = document.getElementById('dragArea');
    let startX = 0;
    let startY = 0;

    container.addEventListener('mousedown', (e) => {
      hidenDragArea();
      startX = e.clientX;
      startY = e.clientY;
      dragArea.style.display = 'block';
      updateDragArea(e);
    });

    container.addEventListener('mousemove', (e) => {
      if (dragArea.style.display !== 'none') {
        updateDragArea(e);
        checkDomInDragArea();
      }
    });

    container.addEventListener('mouseup', () => {
      hidenDragArea();
      //返回被选中dom集合
      this.$emit('selected', container.querySelectorAll('.' + selectClass));
    });

    function hidenDragArea() {
      if (dragArea) {
        dragArea.style.display = 'none';
      }
    }

    function updateDragArea(e) {
      const offsetX = container.offsetLeft; //container到浏览器左侧的偏移
      const offsetY = container.offsetTop; //container到浏览器顶部的偏移
      const width = Math.abs(e.clientX - startX); //根据鼠标开始拖拽的起始点和拖拽过程中的事件坐标进行拖拽区域的计算
      const height = Math.abs(e.clientY - startY);
      const left = Math.min(e.clientX, startX) - offsetX; //在定位的基础上进行修正
      const top = Math.min(e.clientY, startY) - offsetY;
      //绘制
      Object.assign(dragArea.style, {
        width: `${width}px`,
        height: `${height}px`,
        left: `${left}px`,
        top: `${top}px`,
      });
    }

    /**
     * 检查区域内的dom节点
     */
    function checkDomInDragArea() {
      const domElements = container.querySelectorAll('*:not(.drag-area)');
      const dragAreaRect = dragArea.getBoundingClientRect();
      domElements.forEach((element) => {
        const elementRect = element.getBoundingClientRect();
        const isInDragArea =
          elementRect.left >= dragAreaRect.left &&
          elementRect.right <= dragAreaRect.right &&
          elementRect.top >= dragAreaRect.top &&
          elementRect.bottom <= dragAreaRect.bottom;

        if (isInDragArea) {
          element.classList.add(selectClass);
        } else {
          element.classList.remove(selectClass);
        }
      });
    }
  },
};
</script>

<style scoped>
.container {
  position: relative;
  width: 100%;
}
.drag-area {
  position: absolute;
  border: 1px dashed #000;
  background-color: rgba(0, 0, 0, 0.1);
  z-index: 99999;
  display: none;
}
.selected {
  border: 1px solid black;
}
</style>

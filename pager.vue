<template lang="html">
  <ul class="banma-pager" @click="onPagerClick">
    <li class="btn-prev fa fa-chevron-left" :class="{disabled: currentPage === 1}"></li>
    <li :class="{active: currentPage === 1 }" v-if="totalPages > 0">1</li>
    <li
      class="btn-quickprev fa"
      :class="[quickprevIconClass]"
      v-if="showPrevMore"
      @mouseenter="quickprevIconClass = 'fa-angle-double-left'"
      @mouseleave="quickprevIconClass = 'fa-ellipsis-h'">
    </li>
    <li :class="{ active: currentPage === pager }" v-for="pager in pagers">{{pager}}</li>
    <li
      class="btn-quicknext fa"
      :class="[quicknextIconClass]"
      v-if="showNextMore"
      @mouseenter="quicknextIconClass = 'fa-angle-double-right'"
      @mouseleave="quicknextIconClass = 'fa-ellipsis-h'">
    </li>
    <li
      :class="{ active: currentPage === totalPages }"
      v-if="totalPages > 1">{{ totalPages }}</li>
    <li class="btn-next fa fa-chevron-right" :class="{disabled: currentPage === totalPages}"></li>
  </ul>
</template>

<script>
export default {
  name: 'banma-pager',
  prop: {
    currentPage: Number,
    totalPages: Number
  },
  data() {
    return {
      showPrevMore: false,
      showNextMore: false,
      quicknextIconClass: 'fa-ellipsis-h',
      quickprevIconClass: 'fa-ellipsis-h'
    }
  },
  warch: {
    showPrevMore(val) {
      if (!val) this.quickprevIconClass = 'fa-ellipsis-h'
    },
    showNextMore(val) {
      if (!val) this.quicknextIconClass = 'fa-ellipsis-h'
    }
  },
  computed: {
    pagers() {
      const pagerCount = 7

      const currentPage = Number(this.currentPage)
      const totalPages = Number(this.totalPages)

      let showPrevMore = false
      let showNextMore = false

      if (totalPages > pagerCount) {
        if (currentPage > pagerCount - 2) showPrevMore = true
        if (currentPage < totalPages - 2) showNextMore = true
      }

      const array = []
      if (showPrevMore && !showNextMore) {
        const startPage = totalPages - (pagerCount - 2);
        for (let i = startPage; i < totalPages; i++) {
          array.push(i)
        }
      } else if (!showPrevMore && showNextMore) {
        for (let i = 2; i < pagerCount; i++) {
          array.push(i)
        }
      } else if (showPrevMore && showNextMore) {
        const offset = Math.floor(pagerCount / 2) - 1;
        for (let i = currentPage - offset ; i <= currentPage + offset; i++) {
          array.push(i)
        }
      } else {
        for (let i = 2; i < totalPages; i++) {
          array.push(i)
        }
      }

      this.showPrevMore = showPrevMore
      this.showNextMore = showNextMore
      return array
    }
  },
  methods: {
    onPagerClick(event) {
      const target = event.target
      if (target.tagName === 'UL') return

      let newPage = Number(target.textContent)
      const totalPages = this.totalPages
      const currentPage = this.currentPage

      // 前后翻页
      const className = target.className
      if (className.indexOf('btn-') !== -1) {
        if (className.indexOf('btn-quickprev') !== -1) {
          newPage = currentPage - 5
        } else if (className.indexOf('btn-quicknext') !== -1) {
          newPage = currentPage + 5
        } else if (className.indexOf('btn-prev') !== -1) {
          newPage = currentPage - 1
        } else if (className.indexOf('btn-next') !== -1) {
          newPage = currentPage + 1
        }
      }

      if (!isNaN(newPage)) {
        if (newPage < 1) newPage = 1
        if (newPage > totalPages) newPage = totalPages
      }
    }
  },
  components: {}
}
</script>

<style lang="css">
.banma-pager {
	white-space: nowrap;
	background: #fff;
  padding: 0;
	color: #475669;
  display: inline-block;
}
.banma-pager li {
  float: left;
	padding: 0 4px;
	border: 1px solid #d3dce6;
	border-right: 0;
	background: #fff;
	font-size: 13px;
	min-width: 28px;
  height: 28px;
	line-height: 28px;
	cursor: pointer;
	box-sizing: border-box;
	text-align: center
}
.banma-pager li:hover {
	color: #20a0ff
}
.banma-pager li:first-child {
  color: #99a9bf;
  border-radius: 2px 0 0 2px;
}
.banma-pager li:last-child {
  border-right: 1px solid #d3dce6;
  color: #99a9bf;
  border-radius: 0 2px 2px 0;
}
.banma-pager li.active {
	border-color: #20a0ff;
	background-color: #20a0ff;
	color: #fff;
	cursor: default
}
.banma-pager .more .fa {
  display: none;
}
.banma-pager .icon-arrow-left .fa-angle-double-left {
  display: inline-block;
}
.banma-pager .icon-arrow-right .fa-angle-double-right {
  display: inline-block;
}
.banma-pager .icon-more .fa-ellipsis-h {
  display: inline-block;
}
.banma-pager li.disabled {
	color: #e4e4e4;
	background-color: #fff;
	cursor: not-allowed
}
</style>

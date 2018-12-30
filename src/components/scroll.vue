<template>
  <div class="container" @scroll="onContainerScroll" id="anyScroll--container">
    <slot :data="realContent"></slot>
  </div>
</template>

<script lang="ts">
import {
  Component, Prop, Vue, Watch,
} from 'vue-property-decorator';

@Component
export default class Scroll extends Vue {
  public name: string = 'Scroll';

  @Prop({ type: Array, default: () => [] })
  public content!: number[];

  @Prop({ type: Number, default: 10 })
  public count!: number;

  public realContent: number[] = [];

  private start: number = 0;

  get end(): number {
    return this.start + this.count;
  }

  @Watch('content', { immediate: true })
  onContentChange(val: number[]) {
    if (val.length > this.count) {
      this.realContent = val.slice(this.start, this.end);
    } else {
      this.realContent = val;
    }
  }

  @Watch('start')
  onStartChange(val: number) {
    this.realContent = this.content.slice(val, this.end);
  }

  public onContainerScroll(e: Event) {
    const target: HTMLDivElement = (e.target as HTMLDivElement);
    const height = target.offsetHeight;
    const { scrollTop } = target;
    const container$ = document.querySelector('#anyScroll--container');
    if (scrollTop >= height) {
      console.log('滚动到底部啦');
      if (this.start === this.content.length) {
        return;
      }
      this.start += this.count;
      // 重置滚动条到顶部
      if (container$) {
        container$.scrollTop = 1;
      }
      console.log(this.start, this.end);
    } else if (scrollTop === 0) {
      console.log('滚动到顶部啦');
      if (this.start === 0) {
        return;
      }
      this.start -= this.count;
      if (container$) {
        container$.scrollTop = height - 1;
      }
    }
  }
}
</script>

<style lang="scss" scoped>
.container{
  height: 100px;
  overflow: auto;
}
</style>

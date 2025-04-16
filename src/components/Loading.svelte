<span style="display: flex; align-items: center; gap: 10px">
  <div class="ripple">
    <div></div>
    <div></div>
  </div>
</span>

<style lang="scss">
.ripple {
  display: inline-block;
  position: relative;
  width: 80px;
  height: 80px;
  color: var(--site-accent-color);
  box-sizing: border-box;

  div {
    position: absolute;
    border: 4px solid currentColor;
    opacity: 1;
    border-radius: 50%;
    animation: ripple 1s cubic-bezier(0, 0.2, 0.8, 1) infinite;
    box-sizing: border-box;

    &:nth-child(2) {
      animation-delay: -0.5s;
    }
  }
}

@keyframes ripple {
  0% {
    top: 36px;
    left: 36px;
    width: 8px;
    height: 8px;
    opacity: 0;
  }
  4.9% {
    top: 36px;
    left: 36px;
    width: 8px;
    height: 8px;
    opacity: 0;
  }
  5% {
    top: 36px;
    left: 36px;
    width: 8px;
    height: 8px;
    opacity: 1;
  }
  100% {
    top: 0;
    left: 0;
    width: 80px;
    height: 80px;
    opacity: 0;
  }
}
</style>

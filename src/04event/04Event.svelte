<script>
    let m = { x: 0, y: 0 };

    function handleMousemove(event) {
        m.x = event.clientX;
        m.y = event.clientY;
    }

    function handleClick() {
        alert('clicked')
    }

    //调用组件自定义事件
    import Inner from "./04Inner.svelte"
    import Outer from "./04Outer.svelte"

    function handleMessage (event) {
        alert(event.detail.text)
    }

    //事件转发也可以应用到DOM事件。
    import CustomButton from "./04CustomButton.svelte";

    function sayHello () {
        alert("Hello");
    }
</script>

<style>
    div { width: 50%; height: 50%; background-color: chartreuse}
</style>

<!--<div on:mousemove={handleMousemove}>-->
<!--    The mouse position is {m.x} x {m.y}-->
<!--</div>-->

<!--函数内联-->
<div on:mousemove={e => m = {x: e.clientX, y: e.clientY}}>
    The mouse position is {m.x} x {m.y}
</div>

<!--DOM 事件处理程序具有额外的 修饰符（modifiers） 。 例如，带 修饰符的事件处理程序只运用一次-->
<!--所有修饰符列表：
    preventDefault ：在运行处理程序之前调用。 比如，对客户端表单处理有用。event.preventDefault()
    stopPropagation ：防止事件影响到下一个元素。event.stopPropagation()
    passive ： 优化了对 touch/wheel 事件的滚动表现(Svelte 会在合适的地方自动添加滚动条)。
    capture — 在 capture 阶段而不是bubbling 阶段触发事件处理程序 ()
    once ：运行一次事件处理程序后将其删除。
    self — 仅当 event.target 是其本身时才执行。
    你可以将修饰符组合在一起使用，例如：。on:click|once|capture={...}
-->
<button on:click|once={handleClick}>
    click me
</button>

<!--<Inner on:message={handleMessage}/>-->
<!--组件之间的事件不会冒泡，要手动转发-->
<Outer on:message={handleMessage}/>

<!--我们希望<CustomButton>组件内部接收外部的handleClick()，为此，我们只需要为CustomButton.svelte内的<button>标签添加click事件即可-->
<CustomButton on:click={sayHello}/>
# A-Frame学习总结

## 实体-组件-系统构架

>实体（Entities）是容器对象，用来包含组件。实体是场景中所有对象的基础。没有附加组件的实体不会渲染任何东西，类似于空的< div >。

>组件（Components） 是可重用的模块或数据容器，可以依附于实体以提供外观、行为和/或 功能。组件就像即插即用的对象。所有的逻辑都是通过组件实现，并通过混合、匹配和配置组件来定义不同类型的对象。像炼金术那样！

>系统（Systems） 为组件类提供全局范围、管理和服务。系统通常是可选的，但我们可以使用它们 来分离逻辑和数据；系统处理逻辑，组件充当数据。 容器.

**我的理解：**

实体

实体表现在html里面，以< a-name >或者 < a-entity id='name' >来区分它们，name相当于实体的身份标识。

我们可以给实体添加很多不同种类的组件，比如geometry(几何),material(材料),light(光照),sound(声音)。
每个实体里的每个组件可以看成一个对象，如果我们想根据实体用JS设置或者改变这个实体的某个组件的数据，方法和状态。我们可以利用< a-entity>.components这个对象。如：

    var camera = document.querySelector('a-entity[camera]').components.camera.camera;
    var material = document.querySelector('a-entity[material]').components.material.material;
[实体](http://techbrood.com/aframe/core?p=entity)也有自己的状态，方法和DOM事件，比较多，详情阅读链接。


组件

组件既表现在JS里，也表现在HTML里。在JS里很像VUE的组件，如需要注册，有父子关系，还有生命周期等。组件可以重复利用，用来给实体添加外观，行为，功能。

在HTML里，如果这个组件只有一个属性需要设置则看起来像个普通的HTNL属性(id,class,eg..)
   
     <a-entity position="1 2 3"></a-entity>
    
如果这个组件有很多属性需要设置，则像HTML里的内联CSS(style="...")

    <a-entity light="type: point; color: crimson"></a-entity>

组件注册

    AFRAME.registerComponent('foo', {
        schema: {},//模式
        init: function () {},//初始化
        update: function () {},//数据变化
        tick: function () {},//在每个场景渲染循环被调用。用于连续的改变或检查。
        remove: function () {},//组件删除时
        pause: function () {},//每当场景或实体暂停来删除任意背景或动态行为时被调用。当组件从实体中移除或实体脱离场景时也会被调用。用于暂停动态行为。
        play: function () {}//每当场景或实体播放来添加任意背景或动态行为时被调用。组件初始化时也会被调用一次。用于启动或恢复动态行为。
    });
PS：我发现还有很多内容，看文档再详细不过，我就不复制黏贴了，接下来我只说我遇到的疑惑和理解。

系统

系统和组件很像，也有注册，生命周期和方法。我感觉系统有点像组件的类，相同类的组件共用这个类的一些公共方法和属性。如果系统名称与组件名匹配（相同），则组件将具有一个对系统的引用:**this.system**。我们可以通过场景访问实例化的系统：

    document.querySelector('a-scene').systems[systemName];

常用的写法：

        <a-scene>
            <a-assets>
                <!--设置元素-->
                <audio src="https://cdn.aframe.io/basic-guide/audio/backgroundnoise.wav" autoplay
        preload></audio>
                <img id="boxTexture" src="https://i.imgur.com/mYmmbrp.jpg">
            </a-assets>
            <a-box src="#boxTexture" position="0 2 -5" rotation="0 45 45" scale="2 2 2">
             <!--设置动画-->
                <a-animation attribute='position' to='0 2.2 -5' direction='alternate' dur='2000' repea='indefinite'></a-animation>
                <a-animation attribute="scale" begin="mouseenter" dur="300" to="2.3 2.3 2.3"></a-animation>

            </a-box>
        </a-scene>

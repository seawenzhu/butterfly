# Butterflies是什么?

Butterflies是什么是一个基于数据驱动文档的一款JavaScript函数库，让你有能力借助HTML，SVG，CSS来方便快捷生成可视化流程图表。

# 安装
```
npm install butterfly-dag
```

# 使用方法
```
let canvas = new Canvas({
  root: dom,              //canvas的根节点(必传)
  layout: 'ForceLayout'   //布局设置(可传)
  zoomable: true,         //可缩放(可传)
  moveable: true,         //可平移(可传)
  draggable: true,        //节点可拖动(可传)
  linkable: true,         //节点可连接(可传)
  disLinkable: true,      //节点可取消连接(可传)
  theme: {                //主题定制(可传) 
    edge: {
      type: 'Bezier',     //线条类型：贝塞尔曲线，折线，直线
      Class: XXClass      //自己拓展的class
    },
    endpoint: {
      position: []        //限制锚点位置['Top', 'Bottom', 'Left', 'Right']
    }
  }
});
canvas.draw({
  groups: [],  //分组信息
  nodes: [],  //节点信息
  edges: []  // 连线信息
})
```


# API文档

## Canvas

### 属性：

| key | 说明 | 类型 | 默认值 
| :------ | :------ | :------ | :------ 
| root | 渲染画布的跟节点 | Dom (Require) | `*这个dom必须设置position:relative`
| layout | 自动布局 | string (Option) | null 
| zoomable | 画布是否可缩放 | boolean (Option) | false 
| moveable | 画布是否可移动 | boolean (Option) | false 
| draggable | 画布节点是否可拖动 | boolean (Option) | false 
| linkable | 画布节点是否可连接 | boolean (Option) | false 
| disLinkable | 画布节点是否可取消连接 | boolean (Option) | false 
| theme | 画布主题 | object (Option) | false 

### API：
```
/**
  * 渲染方法
  * @param {data} data  - 里面包含分组，节点，连线
  */
draw = (data) => {}

/**
  * 添加分组
  * @param {object|Group} object  - 分组的信息；Group － 分组的基类
  */
addGroup = (object|Group) => {}

/**
  * 根据id获取node
  * @param {string} id  - node id
  * @return {Node} - 节点对象
  */
getNode = (string) => {}

/**
  * 根据id获取group
  * @param {string} id  - group id
  * @return {Group} - 分组对象
  */
getGroup = (string) => {}

/**
  * 根据id获取相邻的edge
  * @param {string} id  - node id
  * @return {Edges} - 相邻的连线
  */
getNeighborEdges = (string) => {}

/**
  * 添加节点
  * @param {object|Node} object  - 节点的信息；Node － 节点的基类
  */
addNode = (object|Node) => {}

/**
  * 添加连线
  * @param {object|Edge} object  - 连线的信息；Edge － 连线的基类
  */
addEdge = (object|Edge) => {}

/**
  * 设置放大缩小
  * @param {true|false} boolean  - 是否支持放大缩小
  */
removeNode = (string) => {}

/**
  * 根据id删除节点
  * @param {string} id  - node id
  * @return {Node} - 删除的对象
  */
removeGroup = (string) => {}

/**
  * 根据id删除分组
  * @param {string} id  - group id
  * @return {Node} - 删除的对象
  */
setZoomable = (boolean) => {}

/**
  * 设置画布平移
  * @param {true|false} boolean  - 是否支持画布平移
  */
setMoveable = (boolean) => {}

/**
  * 聚焦节点或节点组
  * @param {string/function} nodeId/groupId or filter  - 节点的id或者过滤器
  * @param {string} type  - 节点的类型(node or group)
  * @param {function} callback  - 聚焦后的回调
  */
focusNodeWithAnimate = (string, type) => {}

/**
  * 设置框选模式
  * @param {true|false} boolean  - 是否开启框选功能
  * @param {array} type - 可接受框选的内容(node/endpoint/edge,默认node)
  */
setSelectMode = (boolean, type) => {}

/**
  * 获取画布的数据模型
  * @return {data} - 画布的数据
  */
getDataSource = (string) => {}

/**
  * 发送事件
  */
emit = (string, obj) => {}

/**
  * 接受事件
  */
on = (string, callback) => {}
```

### 事件

```
let canvas = new Canvas({...});
canvas.on('type', (data) => {
  //data 数据
});
```

| key | 说明 | 返回 
| :------ | :------ | :------
| system.canvas.click | 点击画布空白处 | -
| system.node.delete | 删除节点 | -
| system.link.delete | 删除连线 | -
| system.link.connect | 连线成功 | -
| system.group.delete | 删除节点组 | -
| system.multiple.select | 框选结果 | -
| system.drag.start | 拖动开始 | -
| system.drag.move | 拖动 | -
| system.drag.end | 拖动结束 | -


## Group
### 用法
```
const Group = require('@ali/butterflies').Group;
class TestGroup extends Group {
  draw(obj) {
    // 这里可以根据业务需要，自己生成dom
  }
}

canvas.draw({
  groups: {
    id: 'xxxx',
    top: 100,
    left: 100,
    Class: TestGroup //设置基类之后，画布会根据自定义的类来渲染
  }
})
```

### 属性

| key | 说明 | 类型 | 默认值 
| :------ | :------ | :------ | :------ 
| id | 节点唯一标识 | string (Require) | - 
| top | y轴坐标 | number (Require) | - 
| left | x轴坐标 | number (Require) | - 
| width | 宽度 | number (Option) | - 
| height | 高度 | number (Option) | - 

`* 节点的返回的dom必须设置position: absolute;`

### 方法
```
/**
  * group的渲染方法
  * @param {obj} data - 节点基本信息 
  * @return {dom} - 返回渲染dom的根节点
  */
draw = (obj) => {}
/**
  * group添加节点
  * @param {obj} node - 节点数据
  */
addNode = (node) => {}
/**
  * group批量添加节点
  * @param {array} nodes - 节点数组
  */
addNodes = (nodes) => {}
/**
  * group删除节点
  * @param {obj} node - 节点数据
  */
removeNode = (node) => {}
/**
  * group删除节点
  * @param {array} nodes - 节点数组
  */
removeNodes = (nodes) => {}
/**
  * 发送事件
  */
emit = (string, obj) => {}

/**
  * 接受事件
  */
on = (string, callback) => {}
```

## Node

### 用法
```
const Node = require('@ali/butterflies').Node;
class TestNode extends Node {
  draw(obj) {
    // 这里可以根据业务需要，自己生成dom
  }
}

canvas.draw({
  nodes: {
    id: 'xxxx',
    top: 100,
    left: 100,
    Class: TestNode //设置基类之后，画布会根据自定义的类来渲染
  }
})
```

### 属性

| key | 说明 | 类型 | 默认值 
| :------ | :------ | :------ | :------ 
| id | 节点唯一标识 | string (Require) | - 
| top | y轴坐标 | number (Require) | - 
| left | x轴坐标 | number (Require) | - 
| group | group的唯一标识 | string (Option) | - 
| endpoints | 锚点信息 | array (Option) | - 

`* 节点的返回的dom必须设置position: absolute;`

### 方法
```
/**
  * 节点的渲染方法
  * @param {obj} data - 节点基本信息 
  * @return {dom} - 返回渲染dom的根节点
  */
draw = (obj) => {}
/**
  * 节点挂载后的回调
  */
mounted = () => {}

/**
  * 删除节点
  */
remove = () => {}

/**
  * 聚焦回调
  */
focus = () => {}

/**
  * 失去聚焦回调
  */
unFocus = () => {}

/**
  * @param {obj} data - 锚点基本信息 
  */
addEndpoint = (obj) => {}

/**
  * @param {string} pointId - 锚点的信息 
  * @return {Endpoint} - Endpoint的对象
  */
getEndpoint = (id) => {}

/**
  * @param {number} x - 移动位置的x坐标 
  * @param {number} y - 移动位置的y坐标 
  */
moveTo = (obj) => {}

/**
  * @return {number} - 节点宽度
  */
getWidth = () => {}

/**
  * @return {number} - 节点高度
  */
getHeight = () => {}

/**
  * 发送事件
  */
emit = (string, obj) => {}

/**
  * 接受事件
  */
on = (string, callback) => {}
```


## 线

### 属性
| key | 说明 | 类型 | 默认值 
| :------ | :------ | :------ | :------ 
| id | 节点唯一标识 | string (Require) | - 
| targetNode | 连接目标节点id | string (Option) | - 
| targetEndpoint | 连接目标锚点id | string (Option) | - 
| sourceNode | 连接源节点id | string (Option) | - 
| sourceEndpoint | 连接源锚点id | string (Option) | - 
| type | 标志线条连接到节点还是连接到锚点 | string (Option) | endpoint/node
| orientationLimit | 线条出口的位置 | array (Option) | - 
| shapeType | 线条的类型 | string (Option) | Bezier/Flow/Straight
| label | 线条上加注释 | string/dom (Option) | -

### 方法

```
/**
  * @return {dom} - 自定义节点的dom
  */
draw = () => {}
/**
  * @return {dom} - 自定义label的dom
  */
drawLabel = () => {}
/**
  * @return {dom} - 自定义箭头的dom
  */
drawArrow = () => {}
/**
  * @param {obj} sourcePoint(可选参数) - 源节点的坐标和方向 
  * @param {obj} targetPoint(可选参数) - 目标节点的坐标和方向 
  * @return {string} - path的路径
  */
calcPath = () => {}
/**
  * 发送事件
  */
emit = (string, obj) => {}
/**
  * 接受事件
  */
on = (string, callback) => {}
```

## 锚点

### 属性
| key | 说明 | 类型 | 默认值 
| :------ | :------ | :------ | :------ 
| id | 节点唯一标识 | string (Require) | - 
| orientation | 方向 | array (Option) | 下:[0,1]/上:[0,-1]/右:[1,0]/左:[-1,0]
| pos | 连接目标锚点id | string (Option) | - 
| scope | 作用域 | string (Option) | 锚点scope相同才可以连线
| type | 目标锚点还是源锚点 | string (Require) | 'source' / 'target'
| root | 可把锚点附属与某个子元素 | string (Option) | - 
| dom | 可以把自定义的子节点 | dom (dom) | - 

### 方法
```
/**
  * @return {dom} - 自定义节点的dom
  */
draw = () => {}
/**
  * @param {number} x - 移动位置的x坐标 
  * @param {number} y - 移动位置的y坐标 
  */
moveTo = (obj) => {}
```

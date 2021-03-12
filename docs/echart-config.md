<style> g{ font-size:26px; color:#CD3333; background: #D8BFD8; border-radius: 5px; opacity: 85%; } </style>

#
# <g>series-pie： </g>🧞
> [!Note|label:饼图：]

<!-- <style> b{ font-size:26px; color: #76EEC6; background: #87CEEB; border-radius: 5px; opacity: 85%; } </style> -->

## roseType 🍭
- 是否展示成南丁格尔图
    - 南丁格尔图：通过半径区分数据大小
    - 可选择两种模式：
        - 'radius' 扇区圆心角展现数据的百分比，半径展现数据的大小。
        - 'area' 所有扇区圆心角相同，仅通过半径展现数据大小。
        
## itemStyle 🌩️
- 图形样式
    ### borderRadius
    - 用于指定饼图扇形区块的内外圆角弧度
    - 支持设置固定数值或者相对于扇形区块的半径的百分比值：
        - borderRadius: 10：表示内圆角半径和外圆角半径都是 10px。
        - borderRadius: '20%'：表示内圆角半径和外圆角半径都是饼图扇形区块半径的 20%。
        - borderRadius: [10, 20]：表示当饼图为环形图时，表示内圆角半径是 10px、外圆角半径是 20px。
        - borderRadius: ['20%', '50%']：表示当饼图为环形图时，内圆角半径是内圆半径的 20%、外圆角半径是外圆半径的 50%。
        <hr>

    > [!Tip|label:设置图形的颜色]

    ### color
    - 图形的颜色
        - 不设置则从全局调色盘获取颜色，显示为各种颜色
        - 设置了则整个图形都是设置的颜色
            - 颜色可以使用 RGB 表示，比如 'rgb(128, 128, 128)'，如果想要加上 alpha 通道表示不透明度，可以使用 RGBA，比如 'rgba(128, 128, 128, 0.5)'，也可以使用十六进制格式，比如 '#ccc'。除了纯色之外颜色也支持渐变色和纹理填充
    <hr>

    > [!Tip|label:设置图形的阴影]

    ### shadowColor
    - 图形阴影的颜色
    - 设置的颜色同color
    ### shadowBlur
    - 图形阴影的模糊大小
    - 该属性配合 shadowColor,shadowOffsetX, shadowOffsetY 一起设置图形的阴影效果。
    ### shadowOffsetX
    - 图形阴影相对图形的水平偏移距离
    ### shadowOffsetY
    - 图形阴影相对图形的垂直偏移距离
    <hr>

    > [!Tip|label:设置图形的轮廓]

    ### borderColor
    - 图形轮廓描边颜色
        - 不设置则无描边
        - 颜色的设置同上方color相同
    ### borderWidth
    - 图形轮廓描边宽度
    ### borderType
    - 图形轮廓描边类型
    - 默认为实线
    - 支持三种类型：
        - solid 实线
        - dashed 虚线
        - dotted 点线
    <hr>
    
    > [!Tip|label:设置图形的透明度]

    ### opacity
    - 图形透明度
    - 支持范围为[0,1]
        - 0为全透明，则无图像
        - 1则是图像本像


😨😰😱😈👿💀☠️🤡👹👺👻👽👾🤖🧡💚🖤🫀🕵️👸🧙🧚
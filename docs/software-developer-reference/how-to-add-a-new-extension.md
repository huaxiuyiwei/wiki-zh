# 如何添加新的设备扩展

## 概述

在 scratch-HW 中 devices extension 是为device设备提供的扩展，不同于scratch原生扩展，devices extension不会被webpack打包，它是由一个外部的扩展服务器提供的，当然这个服务器可以是远程的也可以是本地的。通过这种方式我们就可以实现扩展的动态修改和加载功能了。

## 具体步骤

在scratch-extension-server\src下复制dht11为一个新的文件夹，并将其改名为你的扩展的名字。

### index.js

index.js文件的内容描述了此插件的名称，版本，作者等显示内容，也描述了此插件的积木渲染，代码转译等函数文件的位置。下面以dht11的文件举例。

```js
const formatMessage = require('format-message');
var path = require('path');

const staticPath = path.relative(__dirname + '/../../src', __dirname);

const dht11 = {
    name: "DHT11",
    extensionId: 'dht11',
    version: "1.0.0",
    supportDevice: ['arduinoUno'],
    author: 'Yang',
    iconURL: staticPath + '/asset/DTH11.png',
    description: formatMessage({
        id: 'dht11.description',
        default: 'DHT11 Temperature and humidity sensor module.',
        description: 'Description description of dht11'
    }),
    featured: true,
    blocks: staticPath + '/blocks.js',
    generator:  staticPath + '/generator.js',
    toolbox:  staticPath + '/toolbox.xml',
    msg: staticPath + '/msg.js',
    location: 'local',     // or 'remote'
    link: 'https://www.baidu.com',
}

module.exports = dht11;
```

*以下带有星号\*的参数为必须参数*

- **name***

	显示在扩展选择界面的名字

- **extensionId***

	扩展的唯一ID

- **version**

	显示在扩展选择界面的版本号

- **supportDevice***

	支持的设备，扩展只会选择了对应支持的设备后才会在扩展选择界面显示

- **author**

	显示在扩展选择界面的作者名字

- **iconURL***

	显示在扩展选择界面的图片，图片比例和像素参数请参照DHT11样例中的修改

- **description**

	显示在扩展选择界面图片下方扩展介绍内容

- **featured***

	此参数为内部参数，必须设置为true

- **blocks***

	扩展的积木渲染生成代码

- **generator***

	扩展的积木的转转译代码

- **toolbox***

	扩展的工具箱代码，描述扩展在界面左侧工具箱中的显示和排列方式

- **msg***

	扩展的多语言支持

- **location***

	只有 local / remote 两个选项，用来标识扩展是部署在云端还是本地，在目前的调试版中，直接默认为local 即可

- **link**

	似乎目前还没有使用

### blocks.js

此文件定义了插件内部的积木的样式，以下为DHT样板的代码片段。

```js
// src\dht11\blocks.js
var color = '#28BFE6';

Blockly.Blocks['DHT_INIT'] = {
    init: function () {
        this.jsonInit({
            "message0": Blockly.Msg.DHT_INIT,
            "args0": [{
                "type": "input_value",
                "name": "no"
            }, {
                "type": "field_dropdown",
                "name": "pin",
                "options": [
                    ['0', '0'],
                    ['1', '1'],
                    ['2', '2'],
                    ['3', '3'],
                    ['4', '4'],
                    ['5', '5'],
                    ['6', '6'],
                    ['7', '7'],
                    ['8', '8'],
                    ['9', '9'],
                    ['10', '10'],
                    ['11', '11'],
                    ['12', '12'],
                    ['13', '13'],
                    ['A0', 'A0'],
                    ['A1', 'A1'],
                    ['A2', 'A2'],
                    ['A3', 'A3'],
                    ['A4', 'A4'],
                    ['A5', 'A5']]
            }, {
                "type": "field_dropdown",
                "name": "mode",
                "options": [
                    ['dht11', '11'],
                    ['dht21', '21'],
                    ['dht22', '22']]
            }],
            "colour": color,
            "extensions": ["shape_statement"]
        });
    }
};

// src\dht11\msg.js line 4
DHT_INIT : 'init dht %1 pin %2 mode %3',
```

一个积木块由以下参数构成

**message0** ：

​	积木整体显示内容由message0内容决定，由于这里由多语言翻译内容，所以这里的内容是由 msg.js 中定义的，可以看到积木中的参数是由 %1 %2这样的形式定义的。而后其参数的具体内容由args0定义的数组定义。

**args0** 属性：

- **type**

	可选值：

	​	**input_value**：输入框，默认为圆角矩形，即数字与字符串接收框，如果下方check属性的值为Boolean，则此处为尖角矩形，即布尔值接收框。

	​	**field_dropdown**：下拉框，选项内容在option中定义。

	​	**math_angle**：输入框，数字与字符串接收框，蛋点击后会弹出一个角度界面，可以以图形化方式设定此输入框数值。

	​	... 仍有很多选项，待补充

- **name**

	参数名称，在代码转译函数中会根据这个名称来获取次参数的值。

- **check**

	可选值：

	​	**Boolean**：定义此参数框仅接收布尔值。

	​	**Number**：定义此参数框仅接收数字值。

- **options**

	定义下拉框的内容，数组的第一个值为实际显示内容，第二个值为代码转译时获取的值。

**colour**：

​	积木颜色，一般一个扩展我们通常都使用同一个颜色，所此处以一个颜色变量引入。

**extensions**：

​	这个参数决定了，积木的类型或这说是外部形状。可选值有：

​	**shape_statement**：可以上下连接积木的常规命令方块

​	**output_number**：圆角矩形积木，输出数字值

​	**output_string**：圆角矩形积木，输出字符串

​	**output_boolean**：尖角矩形积木，输出布尔值

​	.. 还有一些选项，待补充

### generator.js

此文件定义了插件积木的代码转译函数。

```js
Blockly.Arduino['DHT_INIT'] = function (block) {
    let no = Blockly.Arduino.valueToCode(this, 'no', Blockly.Arduino.ORDER_ATOMIC);
    let pin = this.getFieldValue('pin');
    let mode = this.getFieldValue('mode');

    Blockly.Arduino.includes_['include_DHT_INIT'] = `#include "<DHT.h>"`;
    Blockly.Arduino.definitions_['define_DHT_INIT_${no}'] = `DHT dht_${no}(${pin}, ${mode});`;
    return '';
}

Blockly.Arduino['DHT_READ_HUMIDITY'] = function (block) {
	let no = Blockly.Arduino.valueToCode(this, 'no', Blockly.Arduino.ORDER_ATOMIC);
	return [`dht_${no}.readHumidity()`, Blockly.Arduino.ORDER_ATOMIC];
}
```

在转译函数中，有两个函数方式来获取积木内部参数：

- **Blockly.Arduino.valueToCode**：

​	此函数用于获取积木内部的可以接收其他积木输入的参数内容（由于这类输入框中可能会放置其他积木，使用该函数会继续调用解析其内容积木内容）。函数第一个参数输入这个积木本身即this，第二个参数为此输入框的名称，第三个参数为参数优先级，直接默认使用Blockly.Arduino.ORDER_ATOMIC即可。

- **this.getFieldValue**

​	此函数用于获取积木内部下拉框参数的内容

积木代码的组成有两个部分，一是定义在开头位置的include等内容，二是在积木本身位置生成的代码内容。对于前者：

- **Blockly.Arduino.includes_、Blockly.Arduino.definitions_、Blockly.Arduino.definitions_、Blockly.Arduino.setups_、Blockly.Arduino.loops_**

	为这几个变量赋值，会在代码对应的位置生成定义的内容。注意这里为数组形式，相同名称的数组内容不会重复生成。放置拖拽同一个积木时生成重复的代码内容。

而对于后者，积木本身位置的代码需要在return中返回。如果积木是一个输出类型，还需要在返回时返回一个代码优先级，此处直接使用Blockly.Arduino.ORDER_ATOMIC即可。

### toolbox.xml

此文件定义了积木在toolbox中的显示内容。以下为DHT样板的代码片段。

```xml
<category name="%{BKY_DHT_CATEGORY}" id="DHT_CATEGORY" colour="#7ED321" secondaryColour="#71BE1E">
    <block type="DHT_INIT" id="DHT_INIT">
        <value name="no">
            <shadow type="math_number">
                <field name="NUM">1</field>
            </shadow>
        </value>
    </block>
    <block type="DHT_READ_HUMIDITY" id="DHT_READ_HUMIDITY">
        <value name="no">
            <shadow type="math_number">
                <field name="NUM">1</field>
            </shadow>
        </value>
    </block>
    <block type="DHT_READ_TEMPERATURE" id="DHT_READ_TEMPERATURE">
        <value name="no">
            <shadow type="math_number">
                <field name="NUM">1</field>
            </shadow>
        </value>
    </block>
</category>
```

此处目录的名称为了多语言翻译，调用了msg中DHT_CATEGORY的值，不过在这里使用时需要在前方添加`BKY_`

而后下方就是之前我们定义的积木的结构，不过需要注意的是，这里我们还需要对积木内部的input_value赋初始值，即代码片段中block内部的value内容。积木下拉菜单中默认值是排第一个的选项，如果我们需要设定该默认值，直接在block结构下设定field即可，如下所示我们对dht11初始化积木的mode选项设定其值为22，这里需要使用实际值而非显示值。

```xml
<block type="DHT_INIT" id="DHT_INIT">
    <value name="no">
        <shadow type="math_number">
            <field name="NUM">1</field>
        </shadow>
    </value>
	<field name="mode">22</field>
</block>
```



## 测试插件

完成修改后我们重启服务器，而后在GUI界面中即可测试这个插件了。
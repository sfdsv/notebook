# opentrons-ot2-ubuntu20.04操作流程记录

## 环境安装

### 安装python

几乎所有的linux系统都默认安装了python。

**要求python版本高于3.7.6 。**

#### 确定已安装的python版本

```bash
luz@luz-Vostro-3470:~$ python --version

找不到命令“python”，您的意思是：

  command 'python3' from deb python3
  command 'python' from deb python-is-python3
```

```bash
luz@luz-Vostro-3470:~$ python3 --version
Python 3.8.10
```

#### 安装python3

如果你想安装其它的python3版本，只需执行几个命令即可。

```bash
$ sudo add-apt-repository ppa:fkrull/deadsnaks
$ sudo apt-get update
$ sudo apt-get intall python3.7
```

启动python3.5的终端会话：

```python
$ python3.7
>>>
```

### 安装pip

大多数较新的Python版本都自带pip。

#### 检查是否安装了pip

```bash
luz@luz-Vostro-3470:~$ pip --version

Command 'pip' not found, but can be installed with:

sudo apt install python3-pip
```

或

```bash
luz@luz-Vostro-3470:~$ pip3 --version

Command 'pip3' not found, but can be installed with:

sudo apt install python3-pip
```

#### 安装pip

```bash
$ sudo apt install python3-pip
```

安装完成后，重新获取pip的版本，显示有版本输出。

```bash
luz@luz-Vostro-3470:~$ pip3 --version
pip 20.0.2 from /usr/lib/python3/dist-packages/pip (python 3.8)
luz@luz-Vostro-3470:~$ pip --version
pip 20.0.2 from /usr/lib/python3/dist-packages/pip (python 3.8
```

### 安装`opentron package`包

```bash
$ pip install opentrons
```

输出显示：
 ```bash
   luz@luz-Vostro-3470:~$ pip install opentrons
   Collecting opentrons
     Using cached opentrons-6.2.1-py2.py3-none-any.whl (1.2 MB)
   Collecting anyio==3.3.0
     Downloading anyio-3.3.0-py3-none-any.whl (77 kB)
        |████████████████████████████████| 77 kB 126 kB/s 
   Collecting jsonschema==3.0.2
     Downloading jsonschema-3.0.2-py2.py3-none-any.whl (54 kB)
        |████████████████████████████████| 54 kB 230 kB/s 
   Collecting opentrons-shared-data==6.2.1
     Downloading opentrons_shared_data-6.2.1-py2.py3-none-any.whl (326 kB)
        |████████████████████████████████| 326 kB 300 kB/s 
   Collecting typing-extensions<5,>=4.0.0
     Using cached typing_extensions-4.4.0-py3-none-any.whl (26 kB)
   Collecting pyserial==3.5
     Downloading pyserial-3.5-py2.py3-none-any.whl (90 kB)
        |████████████████████████████████| 90 kB 100 kB/s 
   Requirement already satisfied: numpy<2,>=1.15.1 in /usr/lib/python3/dist-packages (from opentrons) (1.17.4)
   Collecting pydantic==1.8.2
     Downloading pydantic-1.8.2-cp38-cp38-manylinux2014_x86_64.whl (13.7 MB)
        |████████████████████████████████| 13.7 MB 85 kB/s 
   Collecting aionotify==0.2.0
     Using cached aionotify-0.2.0-py3-none-any.whl (6.6 kB)
   Collecting click<9,>=8.0.0
     Using cached click-8.1.3-py3-none-any.whl (96 kB)
   Requirement already satisfied: idna>=2.8 in /usr/lib/python3/dist-packages (from anyio==3.3.0->opentrons) (2.8)
   Collecting sniffio>=1.1
     Downloading sniffio-1.3.0-py3-none-any.whl (10 kB)
   Collecting pyrsistent>=0.14.0
     Downloading pyrsistent-0.19.3-py3-none-any.whl (57 kB)
        |████████████████████████████████| 57 kB 70 kB/s 
   Requirement already satisfied: setuptools in /usr/lib/python3/dist-packages (from jsonschema==3.0.2->opentrons) (45.2.0)
   Requirement already satisfied: six>=1.11.0 in /usr/lib/python3/dist-packages (from jsonschema==3.0.2->opentrons) (1.14.0)
   Collecting attrs>=17.4.0
     Downloading attrs-22.2.0-py3-none-any.whl (60 kB)
        |████████████████████████████████| 60 kB 78 kB/s 
   Installing collected packages: sniffio, anyio, pyrsistent, attrs, jsonschema, typing-extensions, pydantic, opentrons-shared-data, pyserial, aionotify, click, opentrons
     WARNING: The script jsonschema is installed in '/home/luz/.local/bin' which is not on PATH.
     Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
     WARNING: The scripts pyserial-miniterm and pyserial-ports are installed in '/home/luz/.local/bin' which is not on PATH.
     Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
     WARNING: The scripts opentrons_execute and opentrons_simulate are installed in '/home/luz/.local/bin' which is not on PATH.
     Consider adding this directory to PATH or, if you prefer to suppress this warning, use --no-warn-script-location.
 Successfully installed aionotify-0.2.0 anyio-3.3.0 attrs-22.2.0 click-8.1.3 jsonschema-3.0.2 opentrons-6.2.1 opentrons-shared-data-6.2.1 pydantic-1.8.2 pyrsistent-0.19.3 pyserial-3.5 sniffio-1.3.0 typing-extensions-4.4.0
 ```


发现有几条警告，路径`/home/luz/.local/bin`未在PATH中。

修改`/etc/profile`文件：

```bash
$ sudo gedit /etc/profile
```

![image](https://user-images.githubusercontent.com/73980771/218251083-a9c481d8-04a8-44c2-b6df-3f073db466e0.png)


将`export PATH=$PATH:/home/luz/.local/bin`添加到文件末尾，保存。

注销（不需要重启），重新登录，使配置文件修改生效。

## 工作流程

### 模拟脚本

通常，模拟协议的最佳方法是通过Opentrons应用程序将其上传到OT-2。当您通过应用程序上传协议时，OT-2会模拟协议，应用程序会显示任何错误。

**但是，如果您想在不连接到OT-2的情况下模拟协议，可以下载Opentrons Python包（参考环境配置章节）**。

#### 命令行

一旦Opentrons Python包安装完成，就可以在终端中使用`opentrons_simulate`命令模拟协议：

```bash
$ opentrons_simulate my_protocol.py
```

与Opentrons App类似，模拟器将会打印协议将要执行的动作的日志。如果协议有问题，模拟将停止，并打印错误。

模拟脚本也可以通过python调用：

```bash
$ python3 -m opentrons.simulate /path/to/protocol.py
```

查看`opentrons_simulate`命令选项：

```bash
$ opentrons_simulate --help
```

#### 使用自定义的实验器具

默认的，`opentrons_simulate`将从运行目录中加载自定义的实验器具。

修改`opentrons_simulate`搜索自定义实验器具的目录：

```shell
python.exe -m opentrons.simulate --custom-labware-path="C:\Custom Labware"
```

#### Python脚本

```python
from opentrons.simulate import simulate, format_runlog
# read the file
protocol_file = open('/path/to/protocol.py')
# simulate() the protocol, keeping the runlog
runlog, _bundle = simulate(protocol_file)
# print the runlog
print(format_runlog(runlog))
```

`opentrons.simulate.simulate() `方法模拟协议，返回一个结构化的字典列表的运行日志。

使用`opentrons.simulate.format_runlog()`将日志变得可读。

### 配置和本地存储

`Opentrons Python包`的配置和内部数据存储在用户目录下：`~/.opentrons`。

### API配置流程

一个协议的基本格式如下：

1. 协议制定者、协议的用途
   1. 协议名字
   2. 联系方式
   3. 协议描述
   4. 协议使用的api版本（必要）
2. 告诉机器人在哪里寻找实验器具、移液器和一些硬件模块（可选）
3. 命令机器人控制其连接的硬件

**[示例]**

如果我们需要OT-2在一个板子上从孔A1到孔B1进行移液，协议如下。

```python
from opentrons import protocol_api

# metadata
metadata = {
    'protocolName': 'My Protocol',
    'author': 'Name <opentrons@example.com>',
    'description': 'Simple protocol to get started using the OT-2',
    'apiLevel': '2.13'
}

# protocol run function
def run(protocol: protocol_api.ProtocolContext):

    # labware
    plate = protocol.load_labware('corning_96_wellplate_360ul_flat', location='1')
    tiprack = protocol.load_labware('opentrons_96_tiprack_300ul', location='2')

    # pipettes
    left_pipette = protocol.load_instrument(
         'p300_single', mount='left', tip_racks=[tiprack])

    # commands
    left_pipette.pick_up_tip()
    left_pipette.aspirate(100, plate['A1'])
    left_pipette.dispense(100, plate['B2'])
    left_pipette.drop_tip()
```

上诉代码描述如下信息：

1. 提供协议的名称、联系信息和简短描述。表明协议可以运行的API的最低版本。
2. 告诉机器人有：
   1. 槽1中的96孔平板。
   2. 槽2中有300µL枪头的支架。
   3. 一个单通道300µL移液器连接在左侧挂载点上，该移液管应从上述枪头支架上取枪头。
3. 告诉机器人动作：
   1. 从枪头支架上拾取第一个Tip头。
   2. 从平板的孔A1吸入100µL液体。
   3. 将100µL液体分配到平板的孔B1中。
   4. 把Tip扔到垃圾桶里。

### 命令

#### 基础命令

##### 操作枪头

前置命令：

```python
from opentrons import protocol_api

metadata = {'apiLevel': '2.13'}

def run(protocol: protocol_api.ProtocolContext):
    tiprack = protocol.load_labware('corning_96_wellplate_360ul_flat', 2)
    plate = protocol.load_labware('opentrons_96_tiprack_300ul', 3)
    pipette = protocol.load_instrument('p300_single_gen2', mount='left')
    # the example code below would go here, inside the run function
```

**取枪头**

```python
pipette.pick_up_tip(tiprack['A1'])
```

如果移液器已经和一个枪头支架相关联，那么可以简化调用为：

```python
pipette.pick_up_tip()
```

这将使用tipracks列表中的下一个可用tip头，tipracks列表是被传递的`tip_racks`参数。

**丢枪头**

```python
pipette.pick_up_tip()
pipette.drop_tip(tiprack['A1'])  # 丢枪头到指定位置
pipette.pick_up_tip()
pipette.drop_tip()  # 不指定时，默认丢到垃圾桶
```

**返回枪头**

```python
pipette.pick_up_tip(tiprack['A3'])
pipette.return_tip() # 默认将会返回到位置A3处
```

API版本2.2及以上：

```python
tip_rack = protocol.load_labware(
        'opentrons_96_tiprack_300ul', 1)
pipette = protocol.load_instrument(
    'p300_single_gen2', mount='left', tip_racks=[tip_rack])

pipette.pick_up_tip() # 从位置A1取枪头
pipette.return_tip()
pipette.pick_up_tip() # 从位置B1取枪头
```

API版本2.0和2.1时：

```python
tip_rack = protocol.load_labware(
        'opentrons_96_tiprack_300ul', 1)
pipette = protocol.load_instrument(
    'p300_single_gen2', mount='left', tip_racks=[tip_rack])

pipette.pick_up_tip() # 从位置A1取枪头
pipette.return_tip()
pipette.pick_up_tip() # 从位置A1取枪头
```

**遍历枪头**

前置命令：

```python
from opentrons import protocol_api

metadata = {'apiLevel': '2.13'}

def run(protocol: protocol_api.ProtocolContext):
    plate = protocol.load_labware(
        'corning_96_wellplate_360ul_flat', 2)
    tip_rack_1 = protocol.load_labware(
        'opentrons_96_tiprack_300ul', 3)
    tip_rack_2 = protocol.load_labware(
        'opentrons_96_tiprack_300ul', 4)
    pipette = protocol.load_instrument(
        'p300_single_gen2', mount='left', tip_racks=[tip_rack_1, tip_rack_2])
```
循环取枪头：

```python
pipette.pick_up_tip()  # picks up tip_rack_1:A1
pipette.return_tip()
pipette.pick_up_tip()  # picks up tip_rack_1:A2
pipette.drop_tip()     # automatically drops in trash

# use loop to pick up tips tip_rack_1:A3 through tip_rack_2:H12
tips_left = 94 + 96 # add up the number of tips leftover in both tipracks
for _ in range(tips_left):
    pipette.pick_up_tip()
    pipette.return_tip()
```

当所有被关联的枪头支架的枪头被使用完后，再次取枪头将会引发一个错误：

```python
# this will raise an exception if run after the previous code block
pipette.pick_up_tip()
```

改变移液器取枪头的第一个位置`starting_tip`：

```python
pipette.starting_tip = tip_rack_1.well('C3')
pipette.pick_up_tip()  # 从枪头支架1的C3位置取枪头
pipette.return_tip()
```

重置枪头记录`reset_tipracks()`：

```python
# 用完了所有枪头
for _ in range(96+96):
     pipette.pick_up_tip()
     pipette.return_tip()

# Reset the tip tracker
pipette.reset_tipracks()

# 从第一个枪头支架的A1孔取枪头
pipette.pick_up_tip()
```

检查是否存在枪头`has_tip`：

```python
for block in range(3):
    if block == 0 and not pipette.has_tip:
        pipette.pick_up_tip()
    else:
        m300.mix(mix_repetitions, 250, d)
        m300.blow_out(s.bottom(10))
        m300.return_tip()
```

##### 液体控制

前置命令：

```python
metadata = {'apiLevel': '2.13'}

def run(protocol):
    plate = protocol.load_labware('corning_96_wellplate_360ul_flat', 2)
    tiprack = protocol.load_labware('opentrons_96_tiprack_300ul', 3)
    pipette = protocol.load_instrument('p300_single_gen2', mount='left', tip_racks=[tiprack])
    pipette.pick_up_tip()
    # example code goes here
```

**吸液**

```python
pipette.aspirate(50, plate['A1'], rate=2.0)  # 从孔A1吸液50ul，速度2.0
```

* 参数`location`可以为一个孔(`plate['A1']`)或孔内的一个位置(`plate['A1'].bottom`)
* 参数`rate` is a multiplication factor of the pipette’s default aspiration flow rate. 默认的吸液速度在[Defaults](https://docs.opentrons.com/v2/new_pipette.html#defaults).

可以简化吸液参数：

```python
pipette.aspirate(50)  # 从当前位置吸液20ul
```

**喷液**

与吸液类似，指明吸液体积和位置或仅体积：

```python
pipette.dispense(50, plate['B1'], rate=2.0) # dispense 50uL to plate:B1 at twice the normal rate
pipette.dispense(50)  
```

**吹出**

概念：将移液器的tip中的额外的空气吹出，确保剩余的液滴被排出。

```python
pipette.blow_out()            # blow out in current location
pipette.blow_out(plate['B3']) # blow out in current plate:B3
```

**触碰Tip头**

概念：移动移液器当前附着的tip头到孔位的四个相对边缘，去除可能悬挂在tip上的的液滴。

参数：`touch_tip(location, radius, v_offset, speed)`

```python
pipette.touch_tip()            # touch tip within current location
pipette.touch_tip(v_offset=-2) # touch tip 2mm below the top of the current location
pipette.touch_tip(plate['B1']) # touch tip within plate:B1
pipette.touch_tip(plate['B1'], speed=100) # touch tip within plate:B1 at 100 mm/s
pipette.touch_tip(plate['B1'], # touch tip in plate:B1, at 75% of total radius and -2mm from top of well
                  radius=0.75,
                  v_offset=-2)
```

**混合**

概念：在一个单孔里连续执行多次吸喷指令。

参数：`mix(repetitions, volume, location)`

```python
# mix 4 times, 100uL, in plate:A2
pipette.mix(4, 100, plate['A2'])
# mix 3 times, 50uL, in current location
pipette.mix(3, 50)
# mix 2 times, pipette's max volume, in current location
pipette.mix(2)
```

**吸空气**

概念：在吸液后吸入空气，防止液体从移液器的tip漏出。

参数：`air_gap(volume, height)`

```python
pipette.aspirate(100, plate['B4'])
pipette.air_gap(20)
pipette.drop_tip()
```

#### 功能命令

**延迟一段时间**

可用于孵育等。

```python
protocol.delay(seconds=2)             # delay for 2 seconds
protocol.delay(minutes=5)             # delay for 5 minutes
protocol.delay(minutes=5, seconds=2)  # delay for 5 minutes and 2 seconds
```

**暂停直到被恢复**

概念：暂停协议的执行，直到在Opentrons App上按下`resume`。

```python
from opentrons import protocol_api

metadata = {'apiLevel': '2.13'}

def run(protocol: protocol_api.ProtocolContext):
    # The start of your protocol goes here...

    # The OT-2 stops here until you press resume. It will display the message in
    # the Opentrons App. You do not need to specify a message, but it makes things
    # more clear.
    protocol.pause('Time to take a break')
```

**复位**

```python
from opentrons import protocol_api, types

metadata = {'apiLevel': '2.13'}

def run(protocol: protocol_api.ProtocolContext):
    pipette = protocol.load_instrument('p300_single', 'right')
    protocol.home() # Homes the gantry, z axes, and plungers
    pipette.home()  # Homes the right z axis and plunger
    pipette.home_plunger() # Homes the right plunger
```

**注释**

概念：在协议运行期间，显示信息在Opentrons App里。

```python
from opentrons import protocol_api, types

metadata = {'apiLevel': '2.13'}

def run(protocol: protocol_api.ProtocolContext):
    protocol.comment('Hello, world!')
```

**控制和监控机器人轨道灯**

概念：控制机器人的轨道灯开或关。

```python
from opentrons import protocol_api

metadata = {'apiLevel': '2.13'}

def run(protocol: protocol_api.ProtocolContext):
    # turn on robot rail lights
    protocol.set_rail_lights(True)

    # turn off robot rail lights
    protocol.set_rail_lights(False)
```

检查轨道灯是开还是关：

```python
protocol.rail_lights_on  # returns True when the lights are on,
                         # False when the lights are off
```

**监控机器人门**





#### 复杂命令

### 移液器

```python
from opentrons import protocol_api

metadata = {'apiLevel': '2.13'}

def run(protocol: protocol_api.ProtocolContext):
    # Load a P50 multi on the left slot
    left = protocol.load_instrument('p50_multi', 'left')
    # Load a P1000 Single on the right slot, with two racks of tips
    tiprack1 = protocol.load_labware('opentrons_96_tiprack_1000ul', 1)
    tiprack2 = protocol.load_labware('opentrons_96_tiprack_1000ul', 2)
    right = protocol.load_instrument('p1000_single', 'right',
                                     tip_racks=[tiprack1, tiprack2])
```


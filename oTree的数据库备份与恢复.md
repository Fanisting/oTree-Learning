# oTree的数据库备份与恢复

****

## 快速开始!

### 需要的数据库

实验的数据库：oTree
恢复的数据库：test
恢复的实验app: dbr_base, survey_base

### 需要的数据Class

oTree将实验数据一般保存在: subsession, group, player  

一般而言需要保存并恢复的数据维度是：每个app的Group, Player

### 需要了解的命令

`CMD`+ `PostgreSQL命令`

## 数据库备份

### 备份数据为sql文件

1. `win + R` 打开“运行”窗口

   <img src="https://s2.loli.net/2022/05/19/4XE5om18CtBhrAW.png" alt="image-20220519183354011" style="zoom:33%;" />



2. "运行"窗口中输入`cmd`并回车，得到`cmd`界面

   <img src="https://s2.loli.net/2022/05/19/QLmslqFufeoXSdw.png" alt="image-20220519183417511" style="zoom: 50%;" />

3. 输入`cd "D:\PostgreSQL\14\bin"`来进入**pg_dump.exe**所在的文件夹

   <img src="https://s2.loli.net/2022/05/19/9QUJSmKtPIkzD1l.png" alt="image-20220519163448818" style="zoom: 33%;" />

4. **备份命令**：`pg_dump -h 127.0.0.1 -p 5432 -U postgres -c -f otree_backup.sql oTree`

   `postgres`为数据库的用户名，默认下无需更改

   `otree_backup.sql`为数据库备份的文件名

   `oTree`为需要备份的数据库

   <img src="https://s2.loli.net/2022/05/19/S4QVFjOE7d16Jzs.png" alt="image-20220519163749817" style="zoom:33%;" />

5. 在上图窗口输入口令：也就是账户密码，完成备份！

### 查找备份的数据文件

备份的`otree_backup.sql`文件储存在和`pg_dump.exe`相同的目录下

## 数据库恢复

1. 重复数据库备份的**步骤1-3**，进入文件夹

2. **恢复命令**：`psql -U postgres -f otree_backup.sql test`

   `postgres`为数据库的用户名，默认下无需更改

   `otree_backup.sql`为数据库备份的文件名

   `test`为需要恢复到的数据库

3. 在窗口输入口令：也就是账户密码，完成数据恢复！

## 数据提取

需要的软件：**VS Code+PostgreSQL插件**

### PostgreSQL插件安装

1. 打开VS Code，点击左侧栏目的“扩展”，进入扩展市场

   <img src="https://s2.loli.net/2022/05/19/9orL3S4HVIRj8zK.png" alt="image-20220519165328045" style="zoom: 25%;" />

2. 搜索“PostgreSQL”，安装第一个插件，开发者为**Chris Kolkman**，点击安装。

   安装完成后如下图所示，左侧的边栏中最下面会出现一只大象，代表安装成功。

   <img src="https://s2.loli.net/2022/05/19/OaNBXcfJL1yC7xD.png" alt="image-20220519173121851" style="zoom: 33%;" />

### 数据库连接

3. 点击上方的＋号来创建一个连接

   <img src="https://s2.loli.net/2022/05/19/9ZpO4olNbWCT6R3.png" alt="image-20220519173323231" style="zoom:33%;" />

4. 依次输入一下内容，来帮助连接到数据库

   `host name: 127.0.0.1`

   `User name: postgres`

   `Password`: 你的密码

   `Port`: 默认为5432，无需更改

   `Standard Connection`

   `Database name`

   `Show All Databases`

   `Display name of the database connection`: 你想显示的名字，默认即可

   <img src="https://s2.loli.net/2022/05/19/vyNrzKoT8YxhHFJ.png" alt="image-20220519173417414" style="zoom:33%;" />

   <img src="https://s2.loli.net/2022/05/19/enPLhk9M4St3pVv.png" alt="image-20220519173501241" style="zoom:33%;" />

   <img src="https://s2.loli.net/2022/05/19/xUPmQMuO4dKNzwC.png" alt="image-20220519173539510" style="zoom:33%;" />

   <img src="https://s2.loli.net/2022/05/19/QVk6uora9ZCUGPz.png" alt="image-20220519173621927" style="zoom:33%;" />

   <img src="https://s2.loli.net/2022/05/19/pBw3Sk5UQNhFufC.png" alt="image-20220519173645363" style="zoom:33%;" />

   <img src="https://s2.loli.net/2022/05/19/8MicEJ6Gs3tTRUF.png" alt="image-20220519173721814" style="zoom:33%;" />

   <img src="https://s2.loli.net/2022/05/19/GSys7dcIQPuM5Ce.png" alt="image-20220519173753254" style="zoom:33%;" />

5. 最后我们就连接到了现有的三个数据库：**oTree, postgres, test**

   其中，**oTree**是做实验用的数据库，**test**是恢复数据用的数据库

   <img src="C:\Users\mi\AppData\Roaming\Typora\typora-user-images\image-20220519173934283.png" alt="image-20220519173934283" style="zoom: 50%;" />

### 数据提取

6. 右键单击**test**数据库，选择`New Query`，打开一个文件，输入下面的命令

   <img src="C:\Users\mi\AppData\Roaming\Typora\typora-user-images\image-20220519174051622.png" alt="image-20220519174051622" style="zoom:50%;" />

7. 数据提取命令： `Select * from your_app_database`

   `your_app_database`在本文中有多个表，他遵循"app_name + 数据维度"的格式。详情见下表

   | 数据维度 | App1: dbr_base  | App2: survey_base  |
   | -------- | --------------- | ------------------ |
   | Player   | dbr_base_player | survey_base_player |
   | Group    | dbr_base_group  | survey_base_group  |

   每条命令我们都只能选取其中一个表，e.g: 输入`Select * from dbr_base_group`,右键单击`Run Query`运行代码

   <img src="https://s2.loli.net/2022/05/19/rUTEW94jMbp8eSx.png" alt="image-20220519174538771" style="zoom:33%;" />

   ![image-20220519174649366](https://s2.loli.net/2022/05/19/gnGNBoT9s2U7xuS.png)

8. 数据表保存

   在右侧点击保存按钮，并选择保存的格式（推荐CSV格式）

   <img src="https://s2.loli.net/2022/05/19/uTWYRLFzUgSOl2I.png" alt="image-20220519174804975" style="zoom: 50%;" />

   <img src="https://s2.loli.net/2022/05/19/BxzPOKYb4Sicm6U.png" alt="image-20220519174823701" style="zoom:50%;" />

   得到

   <img src="https://s2.loli.net/2022/05/19/LO2SmadT874gZtE.png" alt="image-20220519174848311" style="zoom: 50%;" />

   单击`ctrl+s`进行保存，将格式改为`file.csv`等任意文件名

9. 接下来只需要重复步骤6-8，保存所有表格即可

## 大功告成！


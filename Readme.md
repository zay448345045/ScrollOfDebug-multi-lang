# ScrollOfDebug 调试卷轴
A scroll that dynamically detects and provides an interface to create classes for the game
动态检测并提供为游戏创建类的界面的卷轴
 [Shattered Pixel Dungeon](https://github.com/00-Evan/shattered-pixel-dungeon) and its mods while running the game.以及运行游戏时的mods。

This repository can be added without impacting the functionality of the implementing project if desired.
如果需要，可以添加此存储库，而不会影响实施项目的功能。

## Installation

1. Obtain a fork of获得分叉 <https://github.com/00-Evan/shattered-pixel-dungeon> that is updated to be at least consistent with更新后至少大于 [v1.0.0](https://github.com/00-Evan/shattered-pixel-dungeon/releases/tag/v1.0.0).

2. Implement Scroll of Debug via one of the following methods described below.
通过下面描述的以下方法之一实现调试卷轴。
3. Ensure the code compiles.
确保代码能够编译。
   * If the directory `shatteredpixel.shatteredpixeldungeon` was changed in the fork, a mass find and replace must be done for the scroll to work properly.
如果目录“shatteredpixel.shatteredpyxeldungeon” 的类名 
在fork中被更改，则必须进行大规模查找和替换才能使卷轴正常工作。

### Via `git pull`通过`git pull`

#### Identify The Correct Branch识别正确的分支

Scroll of Debug has 4 active "implementation" branches based on various places in Shattered's commit history as well as the history of Scroll of Debug itself.Scroll of Debug有4个活动的“实现”分支，这些分支基于Shattered提交历史中的不同位置以及Scroll of Debug本身的历史。

```bash允许命令
git pull https://github.com/zrp200/ScrollOfDebug shpd/VERSION/IMPLEMENTATION
```
Where哪呢:
* `VERSION` is one of `1.0` or `1.3``版本是`“1.0”或“1.3”之一`
* `IMPLEMENTATION` is one of `pc-only` or `apk_support` IMPLEMENTATION是“仅限pc”或apk_support之一

It is possible to upgrade from `1.0` to `1.3` and from `pc-only` to `apk_support` with no Scroll of Debug-related conflicts due to the nature of how the branches are implemented.
由于分支实现的性质，可以从“1.0”升级到“1.3”，从“仅pc”升级到`apk_support`，而不会出现与调试相关的冲突。

If you want to avoid pulling in Scroll of Debug's admittably cursed commit history, add a `--squash` argument to the command.如果你想避免拉入Debug被诅咒的提交历史的滚动，请在命令中添加一个“--squash”参数。

##### Version
Currently, the version ranges supported are:
*
#####版本目前，支持的版本范围包括：
* [[v1.0.0](https://github.com/00-Evan/shattered-pixel-dungeon/releases/tag/v1.0.0),
  [v1.3.0](https://github.com/00-Evan/shattered-pixel-dungeon/releases/tag/v1.3.0))
  (`shpd/1.0/` branches)
* [v1.3.0](https://github.com/00-Evan/shattered-pixel-dungeon/releases/tag/v1.3.0) and newer. (`shpd/1.3/` branches)

*Note*: Scroll of Debug is developed on the latest Shattered by default, so the `1.0` branches may be updated a bit more slowly when features have to be changed during backporting to ensure compatibility.
*注意*：默认情况下，Debug的滚动是在最新的Shattered上开发的，因此在后移植过程中必须更改功能以确保兼容性时，“1.0”分支的更新可能会稍微慢一些。

##### Implementation
There are two variants of Scroll of Debug: the minimal `pc-only` implementation, and the invasive `apk_support` implementation that allows Scroll of Debug to work properly on Android devices.
#####实施Scroll of Debug有两种变体：最小的“仅限pc”实现和侵入性的“apk_support”实现，允许Scroll of Debug在Android设备上正常工作。


###### `pc-only`
The `pc-only` branch is a direct implementation of the project via the subtree method (described later).
######`仅限pc`“仅pc”分支是通过子树方法（稍后描述）直接实现项目。


The only added change is to `GameScene.java`, where the Scroll of Debug is automatically added to the hero's inventory whenever `GameScene` is loaded when the game is in debug mode (`-INDEV` suffix), and removes it automatically if the game is not in debug mode. This specific implementation ensures that Scroll of Debug is always available to the developer while preventing unintended leaks of the scroll to players.
唯一增加的更改是“GameScene.java”，当游戏处于调试模式时，只要加载了“GameScene”（后缀为“-INDEV”），调试卷轴就会自动添加到英雄的资源清册背包中，如果游戏不处于调试模式，则会自动将其删除。这种特定的实现确保了开发人员始终可以使用Scroll of Debug，同时防止了Scroll意外泄漏给玩家。

However, this implementation does not have full functionality in Android builds --- the base implementation of Scroll of Debug will be unable to detect classes in `.apk` builds of the game, causing Scroll of Debug's reflection-based commands (`give`, `spawn`, `set`, `seed`, `use`, `inspect`) to not work properly (though variables may still work).
然而，此实现在Android版本中没有完整的功能——Scroll of Debug的基本实现将无法检测到游戏.apk版本中的类，导致Scroll of Debug基于反射的命令（“give”、“spawn”、“set”、“seed”、“use”、“inspect”）无法正常工作（尽管变量可能仍然有效）。

**`pc-only` implementation is best when**:
* You do not intend to share Scroll of Debug builds with others.
* You want to keep Scroll of Debug confined to one directory.
**`仅在pc上实现是最好的**：*您不打算与他人共享Scroll of Debug版本。*您希望将调试滚动限制在一个目录中。


###### `apk_support` (NEW)
apk支持（新特性）

The `apk-support` implementation spreads out Scroll of Debug files across the implementing repository to ensure that Android builds have full functionality of Scroll of Debug.
“apk支持”实现将Scroll of Debug文件分散到实现存储库中，以确保Android构建具有Scroll of Debug的全部功能。

It moves one of the core files (`PackageTrie`) to `SPD-CLASSES`, amends `PlatformSupport` to allow a custom implementation of class retrieval, and then provides a custom Android implementation of this to ensure classes are read correctly.
它将一个核心文件（“PackageTrie”）移动到“SPD-CLASSES”，修改“PlatformSupport”以允许类检索的自定义实现，然后提供该实现的自定义Android实现以确保正确读取类。

It is entirely incompatible with the subtree-based implementation of Scroll of Debug.
它与基于子树的Scroll of Debug实现完全不兼容。

*Note:* You can upgrade from the `pc-only` implementation (or subtree implementation) to the `apk_support` implementation by pulling in the `apk_support` branch at any time. The `apk_support` branch is based on `pc-only`.
*注意：*您可以随时通过拉入“apk_support”分支从“仅pc”实现（或子树实现）升级到“apk_support”实现。“apk_support”分支仅基于“pc”。


### Via `git subtree`
###通过`git子树`

The master branch of the repository can be directly pulled using git subtrees:
可以使用git子树直接提取存储库的主分支：
```bash
git subtree -P core/src/main/java/com/zrp200/scrollofdebug add https://github.com/zrp200/ScrollOfDebug master
```
It can then also be updated by replacing `add` with `pull` in the previous command.
然后，也可以通过将前一个命令中的“add”替换为“pull”来更新它。

Implementing it this way is effectively identical to pulling in the `shpd/1.3/pc-only` branch, but it will lack the `GameScene.java` changes that let Scroll of Debug actually get added.
以这种方式实现它实际上与引入“shpd/1.3/pc-only”分支完全相同，但它将缺少“GameScene.java”的更改，而这些更改实际上可以添加Scroll of Debug。

As such, it is not the preferred way to implement it, but if you lack Shattered's commit history (or desire a custom method of giving access to scroll of debug in-game) for any reason it may be the only viable option for implementation.
因此，这不是实现它的首选方式，但如果你因任何原因缺乏Shattered的提交历史（或希望有一种自定义方法来访问游戏中的调试滚动），这可能是唯一可行的实现方式。

### Via copy-paste
###通过复制粘贴

There is virtually no difference between the subtree method and straight copy-pasting the files into `core/src/main/java/com/zrp200/scrollofdebug`.
子树方法和直接将文件复制粘贴到`core/src/main/java/com/zrp200/scrollofdebug`中几乎没有区别。

## Usage
使用方法

Scroll of Debug lets the reader create virtually any game-related object with a single command.
Debug滚动允许读者使用单个命令创建几乎任何与游戏相关的对象。

A more actively updated documentation can be found in the
更积极更新的文档可以在 [wiki](https://github.com/Zrp200/ScrollOfDebug/wiki).
找到

The main commands currently are:
目前的主要命令是：
* `give <item> [+<level>]` --- places an item of the corresponding `item` class into your inventory. The level can be optionally specified.
*`give<item>[+<level>]`--将相应`item`类的项目放入您的库存中。可以选择指定级别。

* `spawn <mob> [-p]` --- spawns a given `mob` somewhere on the level. `-p` lets you place the mob.
*`span<mob>[-p]`---在关卡的某个地方生成一个给定的`mob``-p让你把敌人放在那里。

* `affect <buff> [duration]` applies the specified `buff` to a selected mob.
*`affect<buff>[duration]将指定的`buff`应用于选定的mob。

* `seed <blob>` ---
*`种子<blob>`---

* `use <class> <method> [ARGS...]` --- which uses the corresponding method of the provided class. A method can also be appended to the end of the `give`, `spawn`, and `affect` commands to act on the created entity.
*`use<class><method>[ARGS…]`---它使用所提供类的相应方法。方法也可以附加到“give”、“spawn”和“impact”命令的末尾，以对创建的实体进行操作。


* `inspect <class>` gives a list of defined methods and fields for the specified class.
*`inspect<class>`给出了指定类的已定义方法和字段的列表。

* `goto <depth>` --- warps you to the target depth immediately.
*`goto<depth>`会立即将你扭曲到目标深度。

* `help [command]` --- gives an overview on all supported commands. Specifying the command will give more information about that specific command.
*`help[command]`--概述了所有支持的命令。指定命令将提供有关该特定命令的更多信息。

### Autofilled entities
The following classes are autofilled with the corresponding Dungeon object by default.
* `hero` < `Dungeon.hero`
* `level` < `Dungeon.level`
###自动填充实体默认情况下，以下类会自动填充相应的地牢对象。
*“英雄”<`地下城.英雄`
*“等级”<“地下城.等级”`


### Variables
Sometimes we need to run multiple methods on a created object or otherwise store an object for later commands.
###变量
有时我们需要在创建的对象上运行多个方法，或者为以后的命令存储一个对象。

Scroll of Debug supports **variables** to make this possible.
调试卷轴支持**变量**来实现这一点。

#### Storage of Variables
####变量的存储

The result of methods that create objects can be stored by starting the command with `@<variable>` where `<variable>` is the name you want it to be stored as.
创建对象的方法的结果可以通过以“@<variable>”开头的命令来存储，其中“<variable>`是您希望将其存储为的名称。

Alternatively, existing entities can be selected directly by using `@<variable> cell` to select a cell or `@<variable> inv` to select from the inventory.
或者，可以通过使用“@<variable>cell”选择单元格或使用“@<variable>inv”从库存中选择，直接选择现有实体。

#### Usage of variables
Once created, the variable can be used by using `@<variable>` in place of any class or argument of a command.
####变量的使用创建后，可以使用“@<variable>”代替命令的任何类或参数来使用该变量。

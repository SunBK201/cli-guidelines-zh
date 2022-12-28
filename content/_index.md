# 命令行界面设计指南

这是一个[开源指南](https://github.com/SunBK201/cli-guidelines-zh)，可以帮助您编写出更好的命令行程序，采用经典的 UNIX 哲学并对其进行改进，以适应现代生产环境的需要。

## 译者 {#authors}

**SunBK201** 

原项目是 [cli-guidelines](https://github.com/cli-guidelines/cli-guidelines)，译者对其进行了本地化。

[@SunBK201](https://github.com/SunBK201)

<iframe class="github-button" src="https://ghbtns.com/github-btn.html?user=SunBK201&repo=cli-guidelines-zh&type=star&count=true&size=large" frameborder="0" scrolling="0" width="170" height="40" title="GitHub"></iframe>

## 作者 {#authors}

**Aanand Prasad** \
Engineer at Squarespace, co-creator of Docker Compose. \
[@aanandprasad](https://twitter.com/aanandprasad)

**Ben Firshman** \
Co-creator [Replicate](https://replicate.ai/), co-creator of Docker Compose. \
[@bfirsh](https://twitter.com/bfirsh)

**Carl Tashian** \
Developer Advocate at [Smallstep](https://smallstep.com/), first engineer at Zipcar, co-founder Trove. \
[tashian.com](https://tashian.com/) [@tashian](https://twitter.com/tashian)

**Eva Parish** \
Technical Writer at Squarespace, O’Reilly contributor.\
[evaparish.com](https://evaparish.com/) [@evpari](https://twitter.com/evpari)

Design by [Mark Hurrell](https://mhurrell.co.uk/). Thanks to Andreas Jansson for early contributions, and Andrew Reitz, Ashley Williams, Brendan Falk, Chester Ramey, Dj Walker-Morgan, Jacob Maine, James Coglan, Michael Dwan, and Steve Klabnik for reviewing drafts.

<iframe class="github-button" src="https://ghbtns.com/github-btn.html?user=cli-guidelines&repo=cli-guidelines&type=star&count=true&size=large" frameborder="0" scrolling="0" width="170" height="30" title="GitHub"></iframe>

[Join us on Discord](https://discord.gg/EbAW5rUCkE) if you want to discuss the guide or CLI design.



## 前言 {#foreword}

在 20 世纪 80 年代，如果您需要用一台个人计算机做一些事情，在面对 `C:\>` 或 `~$` 时，您需要知道该输入什么。
然而帮助是以厚重的螺旋装订手册的形式提供的，错误信息是不透明的，没有 Stack Overflow 可以拯救您。
但是，如果您有幸能够访问 Internet，那么您可以从 Usenet（一个早期的 Internet 社区）获得帮助，那里面满是和您一样沮丧的人。
他们或许可以帮助您解决问题，或者至少提供一些精神支持。

40 年后，每个人都能够轻而易举地使用计算机，但是代价往往是牺牲了底层终端用户的控制权。在许多设备上，根本没有命令行访问方式，一部分原因是这与“围墙花园”和应用商店公司的利益相悖。

今天的大多数人都不知道命令行是什么，更不用说他们为什么要费心思去使用它了。正如计算机先驱 Alan Kay 在[2017年的一次采访](https://www.fastcompany.com/40435064/what-alan-kay-thinks-about-the-iphone-and-technology-now)中所说，“Because people don’t understand what computing is about, they think they have it in the iPhone, and that illusion is as bad as the illusion that ‘Guitar Hero’ is the same as a real guitar.”。

Kay 的“真正的吉他”不完全是 CLI。他讨论的是提供 CLI 功能的计算机编程方式，这种编程方式远超于在文本文件中编写软件。Kay 的信徒们认为，我们需要打破我们已经使用了几十年的基于文本文件编程的局限。

想象一下未来我们用完全不同的方式来编写计算机程序是一件多么令人兴奋的事情。即使在今天，电子表格仍然是迄今为止最流行的编程语言，而无代码运动正在迅速发展，因为它在试图取代一些抢手的天才程序员。

然而，尽管命令行有其陈旧的、数十年之久的约束和令人费解的怪异，但它依旧是计算机中最 _万能_ 的一角。
它能够让您拉开计算机真正的面纱，看看里面到底发生了什么，并能够让您以 GUI 无法承受的复杂与深度与机器进行交互。
任何想学习它的人都可以在几乎所有的笔记本电脑上使用它。
它可以交互运行，也可以自动化运行。
此外，它的变化速度不如系统的其他部分快。
其创造性价值源于稳定性。

所以，当我们仍然拥有命令行的时候，我们应该尽量最大化命令行的效用和可访问性。

从很早之前开始，我们的计算机编程方式发生了很大的变化。
过去的命令行是 _机器优先_ 的: 和脚本平台上的 REPL 差不多。
但是，随着通用解释语言的蓬勃发展，shell 脚本的作用已经减少。
如今的命令行是 _以人为本_ 的: 一个基于文本的 UI，提供对各种工具、系统和平台的访问。
在过去，编辑器是在终端里的，如今的终端通常是编辑器的一个功能。
类似 `git` 的多工具命令也在大量涌现。
嵌套命令，以及执行整个工作流而不是原子功能的高级命令也相继出现。

受传统 UNIX 哲学的启发，受有趣且功能强大的 CLI 环境的驱动，以及在我们作为程序员的经验的指导下，我们决定是时候重温构建命令行程序的最佳实践和设计原则了。

命令行万岁！

## 介绍 {#introduction}

本文档涵盖了高层次的设计理念和具体的指导方针。
它在指导方针上更加偏重，因为作为实践者，我们的原则是不需要进行太多的高谈阔论。
我们相信要通过例子学习，因此我们也准备了不少范例以供大家参考。

这篇指南不会包含类似 emacs 和 vim 那样全屏终端程序。
全屏程序是小众项目，很少有人能设计出这样的项目。

一般来说，本指南也不涉及编程语言和工具。

本指南适用于谁？
- 如果您正在创建一个 CLI 程序，并且您正在为它的 UI 设计寻找原则和具体的最佳实践，那么本指南就是为您准备的。
- 如果您是专业的“CLI UI设计师” ，那就太好了，我们很乐意向您学习。
- 如果您想避免与 40 年来 CLI 设计惯例相悖的错误，本指南非常适合您。
- 如果您想通过程序的良好设计和有用的帮助文档令用户满意，那么本指南绝对适合您。
- 如果您正在创建一个 GUI 程序，这篇指南不适合您，但是如果您仍然决意要阅读，您可能会学到一些 GUI 的错误范式。
- 如果您正在设计 Minecraft 的沉浸式全屏 CLI 端口，那么本指南不适合您。
  (但我们已经等不及要看了!)

## 原则 {#philosophy}

以下是我们认为优秀的 CLI 设计应具备的基本原则。

### 以人为本 {#human-first-design}

传统的 UNIX 命令是为了被其它程序调用而编写的。
与图形应用程序相比，它们与编程语言中的函数有更多的共同之处。

如今，尽管许多 CLI 程序主要(甚至专门)是被人类使用，而不是被程序调用，但它们的许多交互设计仍然承载着过去的包袱。
现在是时候摆脱这些历史包袱了: 如果一个命令主要是被人类所使用的，而不是程序，那么它就应该首先以人为本设计。

### 小即是美 {#simple-parts-that-work-together}

[最初的UNIX哲学](https://en.wikipedia.org/wiki/Unix_philosophy)中一个核心原则：
使用具有整洁接口的简单程序从而组合构建出更大的系统。
您应该编写足够模块化的程序，以便根据需要进行组合重构，而不是不断地把越来越多的新特性塞到一个程序中去。

在过去，程序的组合主要是通过管道和 shell 脚本来实现的。
随着通用解释语言的兴起，它们的作用可能已减弱，但它们并没有消失。
此外，以 CI/CD、业务流和配置管理形式的大型自动化系统蓬勃发展。程序的组合性与以往一样重要。

幸运的是，UNIX 环境中长期建立的约定(专为这一目的而设计)至今仍对我们有帮助。
标准的 in/out/err, signals, exit codes 和其他机制可确保不同的程序很好地连接在一起。
普通基于行的文本很容易在命令之间进行管道连接。
JSON 是一项较新的发明，它在需要时为我们提供了更多结构，并使我们可以更轻松地将命令行工具与 Web 集成。

无论您要构建什么软件，人们绝对会以您意想不到的方式使用它。
您的软件 _将_ 成为一个更大系统中的一部分，您唯一的选择只能是能否让它成为这个系统中行为稳定的一部分。

最重要的是，组合性的设计没有与以人为本的设计相悖。
本文中的大部分建议都是关于如何同时实现这两者。

### 程序间一致性 {#consistency-across-programs}

终端的约定已牢牢扎根于我们的手指。
我们在前期必须支付成本来学习命令行语法、flags、环境变量等等，但只要程序与程序之间的规则模式是一致的，我们付出的学习成本就会在长期的效率方面得到回报。

因此，CLI 应尽可能地遵循现现存的规则模式。
这也是 CLI 这么符合直觉性和易猜性的原因，也是 CLI 用户如此高效的原因。

话虽如此，有些时候程序间一致性会与易用性冲突。
比如，许多历史悠久的 UNIX 命令默认情况下不会输出太多信息，这可能会给那些不太熟悉命令行的人们造成混乱或担忧。

当遵守程序间一致性原则会损害程序的可用性时，我们可以考虑是否要打破这种一致性原则，但这样的决定应该谨慎做出。

### 输出适中 {#saying-just-enough}

终端是一个纯信息的世界。
您可以认为信息是一个界面，就像任何界面一样，经常有过多或过少的信息。

在命令挂起几分钟的期间，由于输出的信息太少，用户会开始怀疑它是否挂掉了。
当命令输出了一页又一页的调试信息时，过多的信息反而淹没了在松散碎片中真正重要的部分。
最终结果一样: 缺乏清晰度，让用户感到困惑和恼火。

这种输出适中的平衡很难做到，但如果软件要为用户提供服务，这绝对是至关重要的。

### 功能易发现性 {#ease-of-discovery}

当涉及到功能的易发现性时，GUI 占了上风。
您能做的一切都显示在您面前的屏幕上，因此您无需学习任何内容就可以找到所需的功能，甚至可以发现自己不知道的功能。

或许您可以假设命令行界面与此相反，您不得不记住所有的功能。
最初的 [Macintosh 人机界面指南](https://archive.org/details/applehumaninterf00appl)，发布于1987年，推荐使用 “看见并指向（而不是记住并键入）” 方式，其中的说法默认您似乎只能选择其中一种方式。

但这些事情不必相互排斥。
使用命令行的效率来自于记住命令，但是没有理由命令本身不能帮助您学习和记忆命令。

具备功能易发现性的 CLI 应包含全面的帮助文档，提供许多示例，并能够对下一步要运行什么命令以及发生错误时应采取的措施提出建议。
我们可以从 GUI 中窃取想法，以使 CLI 易于学习和使用，即使对于进阶用户也是如此。

_引用: The Design of Everyday Things (Don Norman)，Macintosh Human Interface Guidelines_

### 对话是常态 {#conversation-as-the-norm}

GUI 的设计，尤其是在早期，大量使用了 _比喻手法_: 桌面、文件、文件夹、回收站。
这是很有道理的，因为计算机正不断使自己更加符合常识。
比喻的易于实现是 GUI 相对于 CLI 的巨大优势之一。
然而，很讽刺的是，CLI 一直体现了一个偶然的比喻: 它是一场对话。

除了最简单的命令之外，运行一个程序通常涉及不止一次的调用。
通常，这是因为很难在第一次使用命令就正确地达成目标：用户键入命令，遇到错误，修改命令，遇到其他错误等等，直到可以正常使用。
通过反复失败来学习的这种模式就像是一场用户与程序之间的对话。

不过，试错法并不是对话互动的唯一方式。
此外还有：

- 运行一个命令来设置一个工具，然后学习运行哪些命令才能真正开始使用它。
- 运行几个命令来进行一个前置操作，然后运行最后一个命令（比如 多个 `git add`，后面跟着一个 `git commit`）。
- 探索一个系统，例如用许多 `cd` 和 `ls` 来了解目录结构，或使用 `git log` 和 `git show` 来查看文件的历史。
- 在真正运行一个复杂的操作之前，先进行一次演习。

承认命令行交互的对话性意味着您可以将相关技术应用到命令行的设计中。
当用户输入无效时，您可以提出纠正建议; 当用户经历多个步骤的过程时，您可以明确中间状态; 您可以在用户做一些骚操作之前向他们确认一切是否进行正常。

用户正在与您的软件进行对话，不管您是否有意为之。
在最坏的情况下，这是一场充满敌意的对话，让他们感到愚蠢和怨恨。
在最好的情况下，这是一场愉快的交流，可以加速他们获得新知识和成就感的过程。

_深入阅读: [The Anti-Mac User Interface (Don Gentner and Jakob Nielsen)](https://www.nngroup.com/articles/anti-mac-interface/)_

### 鲁棒性 {#robustness-principle}

鲁棒性既是一种客观属性，也是一种主观属性。
当然，软件 _应该_ 是健壮的: 异常的输入应该被妥善处理，操作应该在可能的情况下是幂等的，等等。
但它也应该被 _感觉_ 是健壮的。

您应该让您的软件在感官上不会崩溃。
让它能给人一种即时响应的感觉，就好像它是一个巨大的机械机器，而不是一个脆弱的塑料“软开关”。

主观的鲁棒性需要关注细节，并认真思考可能出错的地方。
这里有许多小细节: 让用户知道当前正在发生什么，解释常见错误的含义，不打印输出可怕的堆栈跟踪信息。

通常，鲁棒性也可以来自简单性。
许多边缘情况和复杂的代码往往会使程序变得脆弱。

### 同感共情 {#empathy}

命令行工具是程序员的创作工具，所以使用起来应该让人感到愉悦。
这并不意味着要把它们变成一款电子游戏，或者使用大量的表情符号（尽管表情符号本身并没有什么错 😉）。
您需要要让用户觉得您是站在他们那一边的，您希望用户一切顺利，并且您已经充分考虑过他们的问题以及解决问题的方案。

虽然这个建议可能会给您带来一些帮助，但您所能采取的行动并不能确保用户会有这种感觉。
取悦用户意味着每一次都 _超过他们的期望_，这始于同感共情。

### 混乱 {#chaos}

这个世界的终端一团糟，
前后矛盾无处不在，拖慢了我们的脚步，让我们不得不反思自己。

然而，不可否认的是，这种混乱一直是力量的源泉。
终端，就像通用的 UNIX 下的计算环境一样，对您可以构建的内容设置很少的限制。
在这个领域里，各种各样的发明都如雨后春笋般涌现。

很讽刺的是，本文档恳请您遵循现有模式，并提供与数十年来命令行传统相悖的建议。
我们和其他人一样对违反规则感到内疚。

但也许您是时候该打破规矩了。
有明确目的要这样做。

> “当一个标准明显损害了生产力或用户满意度时，那么是时候放弃这个标准了” — Jef Raskin, [The Humane Interface](https://en.wikipedia.org/wiki/The_Humane_Interface)

## 指南 {#guidelines}

以下是一系列可以使您的命令行程序进一步优化的特定操作。

第一部分包含您需要遵循的基本内容。
如果这一部分犯错，您的程序会很难用。

其余的部分也都值得去学习。
如果您有时间和精力去添加这些东西，您的程序会比其他一般的程序好很多。

如果您不打算花太多精力在程序的设计上，那么：只需遵循这些规则，您的程序就可能很好。 
另一方面，如果您考虑过并确定以下规则对您的程序是错误的，那也是可以的。
(没有一个权威会因为没有遵循任意规则而拒绝您的程序。)

另外，这些规则不是一成不变的。
如果您有充分的理由不同意这些规则，您可以[提出建议](https://github.com/cli-guidelines/cli-guidelines).

### 基础 {#the-basics}

这里有一些您需要遵守的基本规则。
弄错这些，您的程序要么使用体验很差，要么就会彻底崩溃。

**请尽量使用命令行参数解析库.**
命令行参数解析库要么是语言内置的，要么是优秀的第三方解析库。
这些解析库通常会以一种合理的方式处理参数、解析选项、提供帮助文本，甚至拼写建议。

以下是一些常见的解析库：
* Go: [Cobra](https://github.com/spf13/cobra), [cli](https://github.com/urfave/cli)
* Java: [picocli](https://picocli.info/)
* Node: [oclif](https://oclif.io/)
* Python: [Click](https://click.palletsprojects.com/), [Typer](https://github.com/tiangolo/typer)
* Ruby: [TTY](https://ttytoolkit.org/)
* Rust: [clap](https://clap.rs/), [structopt](https://github.com/TeXitoi/structopt)
* PHP: [console](https://github.com/symfony/console)

**成功时返回 0 退出码，失败时返回非 0 退出码.**
退出码是描述一个程序执行成功还是失败的方式，因此您应该正确地报告这一点。
此外，应将非 0 的退出码应该映射到最重要的失败模式。

**将 output 发送到 `stdout`.**
您命令的主要的 output 应该是 `stdout`。
任何机器可读的内容也应该转到 `stdout` —这是管道默认发送的地方。

**将 messaging 发送到 `stderr`.**
日志消息，错误等都应发送到 `stderr`。
这意味着当命令通过管道传输时，这些消息将显示给用户，而不是馈入下一个命令。

### 帮助 {#help}

**当命令的调用没有传入任何 option 时, 或者用户输入 `-h`, 或 `--help` 时, 显示帮助文本.**

**默认显示精简的帮助文本。**
如果可以，尽量在运行 `myapp` 或 `myapp subcommand` 时显示帮助文本。
除非您的应用程序非常简单，并且默认情况下会执行明确的操作 (e.g. `ls`，或者您的程序是交互式读取输入的 (e.g. `cat`)。

精简的帮助文本应该只包含:

- 有关程序功能的说明。
- 一到两个调用示例。
- 选项的描述，除非有很多选项。
- 提示通过 `--help` 可以获取更多信息。

`jq` 是一个很好的典范。
当您输入 `jq` 时，会显示描述信息和使用示例，然后提示您可以通过 `jq --help` 获取完整的选项列表：

```
$ jq
jq - commandline JSON processor [version 1.6]

Usage:
    jq [options] <jq filter> [file...]
    jq [options] --args <jq filter> [strings...]
    jq [options] --jsonargs <jq filter> [JSON_TEXTS...]

jq is a tool for processing JSON inputs, applying the given filter to
its JSON text inputs and producing the filter's results as JSON on
standard output.

The simplest filter is ., which copies jq's input to its output
unmodified (except for formatting, but note that IEEE754 is used
for number representation internally, with all that that implies).

For more advanced filters see the jq(1) manpage ("man jq")
and/or https://stedolan.github.io/jq

Example:

    $ echo '{"foo": 0}' | jq .
    {
        "foo": 0
    }

For a listing of options, use jq --help.
```

**通过 `-h` 和 `--help` 显示完整的帮助文本.**
以下的输入都应该显示帮助信息:

```
$ myapp
$ myapp --help
$ myapp -h
```

如果一些 flag 和 argument 没有出现在默认帮助文本中，那么应该可以在该 flag 或 argument 的后面通过 `-h` 获取该 flag 或 argument 的帮助信息。

如果您的程序是 `git-like` 程序，以下的情况也应提供帮助文本：

```
$ myapp help
$ myapp help subcommand
$ myapp subcommand --help
$ myapp subcommand -h
```

**提供 feedback 和 issues 的途径.**
一个网站或 GitHub 链接出现在顶级帮助文本中，这是很常见的做法。

**在帮助文本中提供 Web 版的文档链接.**
如果您有关于子命令的特定说明页面或锚点，那就直接加入到帮助中。
如果在 Web 上有更详细的说明文档，或者对一些命令参数进行进一步解释，则此功能特别有用。

**提供使用示例.**
用户倾向于使用示例而不是其他形式的文档，因此请首先在帮助页面中显示使用示例，尤其是常见的一些用法。同时，也可以进一步解释此示例的功能。

您可以通过一系列示例为复杂的使用打下基础。
<!-- TK example? -->

**如果您有大量示例，请将它们放在其他地方,** 例如 Web 上.
拥有详尽的高级示例很有帮助，但是最好不要让帮助文本太长。

对于更复杂的用例，例如与需要与其他工具集成，您可能需要编写完整的教程。

**不要去管 man page.**
如果您遵循这些准则提供文档和帮助，那么根本就不需要 man page。
没有多少人使用 man page，并且 man page 在 Windows 上也无法使用。
如果您的 CLI 框架和程序包管理器使输出 man page 变得很容易，那么可以加入 man page，否则，您最好把时间用在改进 Web 文档和内置帮助文本上去。

_引用: [12 Factor CLI Apps](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46)._

**如果您的帮助文本长, 通过管道输入到分页工具里去.**
`man` 就是这么做的。
参见下面的“输出”一章。

**在帮助文本的开头显示最常用的 flags 和 commands.**
当有许多 flags 的时候，首先显示最常用的部分 flags。
例如，Git 命令首先显示用于入门的命令以及最常用的子命令：

```
$ git
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone      Clone a repository into a new directory
   init       Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add        Add file contents to the index
   mv         Move or rename a file, a directory, or a symlink
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect     Use binary search to find the commit that introduced a bug
   grep       Print lines matching a pattern
   log        Show commit logs
   show       Show various types of objects
   status     Show the working tree status
…
```

**在帮助文本中使用不同格式.**
加粗的标题能够更加容易被发现。
但是，应尽量以独立于终端的方式进行，这样用户就不会盯着一堆转义字符了。

<pre>
<code>
<strong>$ heroku apps --help</strong>
list your apps

<strong>USAGE</strong>
  $ heroku apps

<strong>OPTIONS</strong>
  -A, --all          include apps in all teams
  -p, --personal     list apps in personal account when a default team is set
  -s, --space=space  filter by space
  -t, --team=team    team to use
  --json             output in json format

<strong>EXAMPLES</strong>
  $ heroku apps
  === My Apps
  example
  example2

  === Collaborated Apps
  theirapp   other@owner.name

<strong>COMMANDS</strong>
  apps:create     creates a new app
  apps:destroy    permanently destroy an app
  apps:errors     view app errors
  apps:favorites  list favorited apps
  apps:info       show detailed app information
  apps:join       add yourself to a team app
  apps:leave      remove yourself from a team app
  apps:lock       prevent team members from joining an app
  apps:open       open the app in a web browser
  apps:rename     rename an app
  apps:stacks     show the list of available stacks
  apps:transfer   transfer applications to another user or team
  apps:unlock     unlock an app so any team member can join
</code>
</pre>

Note: When `heroku apps --help` is piped through a pager, the command emits no escape characters.

**如果用户进行了错误操作，您可以猜测用户想要做什么，并提出建议.**
比如, `brew update jq` 告诉用户应该运行 `brew upgrade jq`.

您可以询问用户是否想运行建议的命令，但不要强迫他们这样做。
比如:

```
$ heroku pss
 ›   Warning: pss is not a heroku command.
Did you mean ps? [y/n]:
```

您可能会想，与其建议他们使用正确的语法，不如直接为他们运行，就好像他们一开始就打对了一样。
有时这是正确的，但并不总是这样。

首先，无效的输入并不一定意味着简单的键入错误--它往往意味着用户犯了一个逻辑错误，或者误用了一个 shell 变量。
要假设用户的意图可能是危险的，特别是如果由此产生的操作修改了相关状态。

其次，要注意，如果您改变了用户输入的内容，他们就不会学习到正确的语法。
实际上，您是在裁定他们输入的方式是否有效和正确，而且您承诺将无限期地支持这种方式。
在做出这个决定时，要有意识地记录两种语法。

_Further reading: [“Do What I Mean”](http://www.catb.org/~esr/jargon/html/D/DWIM.html)_

**如果您的命令希望有东西被 pipe 到它那里，且 `stdin` 是一个交互式终端，请立即显示帮助并退出.**
这意味着它不是一直在挂起, 像 `cat` 那样.
另外，您可以打印一条日志信息到 `stderr`.

### 文档 {#documentation}

[帮助文本](#help)的目的是给人一种简短、直接的感觉，让人知道您的工具是什么，有哪些选项，以及如何执行最常见的任务。
另一方面，文档是深入了解全部细节的地方。
人们可以通过文档来了解您的工具可以用来做什么，不可以做什么，它是如何工作的，以及使用全部功能的方法。 

**提供 Web 文档.**
人们可以通过搜索引擎来查询到您提供的文档，并能够通过索引链接到特定的部分。Web 文档是目前最具包容性的文档格式。

**提供终端文档.**
终端文档有几个不错的特性：访问速度快，与工具的安装版本保持同步，而且不需要互联网连接也能工作。

**考虑是否提供 man pages.**
[man pages](https://en.wikipedia.org/wiki/Man_page), Unix 最初的文档系统, 至今仍在使用, 许多用户在尝试了解您的工具时会反射性地查看 `man mycmd`.
为了让 man pages 跟容易生成，您可以使用一个工具[ronn](http://rtomayko.github.io/ronn/ronn.1.html)（它也可以帮助您自动生成 Web 文档）

然而，不是每个人都知道 `man`，而且 `man` 也并非在全平台都可以运行，所以您应该确保终端文档同样可以获取到。
比如，`git` 和 `npm` 可以通过 `help` 子命令获取到 man pages，因此 `npm help ls` 等同于 `man npm-ls`。

```
NPM-LS(1)                                                            NPM-LS(1)

NAME
       npm-ls - List installed packages

SYNOPSIS
         npm ls [[<@scope>/]<pkg> ...]

         aliases: list, la, ll

DESCRIPTION
       This command will print to stdout all the versions of packages that are
       installed, as well as their dependencies, in a tree-structure.

       ...
```

### 输出 {#output}

**人类可读的输出是至关重要的.**
要把人放在首位，机器放在第二位。
对于一个特定的输出流(`stdout` or `stderr`)，要判断它是否被人类读取，最简单且直接的启发方法是看它 _是否是一个 TTY_.

_Further reading on [what a TTY is](https://unix.stackexchange.com/a/4132)._

**在不影响可用性的地方要有机器可读的输出.**
文本流是 UNIX 中的通用接口。
程序通常会输出文本行，而程序通常希望有文本行作为输入，
因此您可以把多个程序编织在一起。
这样做通常是为了让编写脚本成为可能，
但它也有助于人类使用程序的易用性。
比如，用户应该能够用管道输出到 `grep`，它应该做他们所期望的事情。

> “Expect the output of every program to become the input to another, as yet unknown, program.”
— [Doug McIlroy](https://homepage.cs.uri.edu/~thenry/resources/unix_art/ch01s06.html)

**如果人类可读的输出破坏了机器可读的输出，请使用 `--plain` 以纯表格文本格式显示输出，以便与 `grep` 或 `awk` 等工具集成。**
在某些情况下，您可能需要以不同的方式输出信息以使其易于阅读。
例如，如果您正在显示一个基于行的表格，您可能会选择将一个单元格分成多行，以适应更多信息，同时将其保持在屏幕宽度内。
但这打破了每行一条数据的预期行为，因此您应该为脚本提供一个 `--plain` 选项，它可以禁用所有此类操作并每行输出一条记录。

**如果被传入 `--json` ，则输出显示为格式化的 JSON。.**
JSON 比纯文本有更多的结构，所以它能够更容易输出和处理复杂的数据结构。
[`jq`](https://stedolan.github.io/jq/) 是在命令行中处理 JSON 的常用工具, 现在具有一个[完整的工具生态系统](https://ilya-sher.org/2018/04/10/list-of-json-tools-for-command-line/) 可以输出和操作 JSON.

它在 Web 上也被广泛使用，因此通过使用 JSON 作为程序的输入和输出，使您可以使用 `curl` 直接与 Web 服务进行管道传输。

**成功时显示输出，但要保持简短.**
传统上，当一切正常时，UNIX 命令不会向用户显示任何输出。 
这在脚本中是有意义的，但在被人类使用时可能会使命令看起来挂起或损坏。 例如，`cp` 不会打印任何内容，即使它需要很长时间。

什么都不打印是最好的默认行为，这种做法是少见的，但是对于错误信息来说，这种行为是常见的。

对于不需要输出的情况（例如，在 shell 脚本中使用时），为了避免将 `stderr` 笨拙地重定向到 `/dev/null`，您可以提供一个 `-q` 选项来抑制所有非必要的输出。

**如果状态改变了，请告诉用户.**
当命令改变了系统状态时，解释刚刚发生的事情特别有必要，这样用户就可以在脑海中对系统状态进行建模——特别是如果结果没有直接映射到用户请求内容的情况。

比如，`git push` 可以准确地告诉用户目前在做什么，以及远程分支的最新状态：

```
$ git push
Enumerating objects: 18, done.
Counting objects: 100% (18/18), done.
Delta compression using up to 8 threads
Compressing objects: 100% (10/10), done.
Writing objects: 100% (10/10), 2.09 KiB | 2.09 MiB/s, done.
Total 10 (delta 8), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (8/8), completed with 8 local objects.
To github.com:replicate/replicate.git
 + 6c22c90...a2a5217 bfirsh/fix-delete -> bfirsh/fix-delete
```

**让查看系统的当前状态更加容易.**
如果您的程序进行了很多复杂的状态变化，而且在文件系统中不能立即看到，请确保能够使之易于查看。

比如, `git status` 能够告诉您尽可能多的关于 Git 仓库当前状态的信息，以及如何修改这些状态的提示：

```
$ git status
On branch bfirsh/fix-delete
Your branch is up to date with 'origin/bfirsh/fix-delete'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   cli/pkg/cli/rm.go

no changes added to commit (use "git add" and/or "git commit -a")
```

**建议用户应该运行的命令.**
当多个命令形成一个工作流时，向用户建议他们接下来可以运行的命令有助于他们学习如何使用您的程序并发现新功能。
例如，在上面的 `git status` 的输出中，向您提出可以修改当前状态的命令。

**跨越程序内部世界边界的动作通常应该是明确的.**
这包括:

- 读取或写入用户未明确作为参数传递的文件（除非这些文件正在存储内部程序状态，例如缓存）。
- 与远程服务器进行会话，例如下载文件。

**增加信息密度---使用 ASCII 艺术！**
例如, `ls` 以一种清晰且详细的方式显示文件的权限.
当您第一次看到这样的输出时，您可以忽略大多数信息。
然后，当您了解它的工作原理时，您会随着时间的推移挑选出更多的模式。

```
-rw-r--r-- 1 root root     68 Aug 22 23:20 resolv.conf
lrwxrwxrwx 1 root root     13 Mar 14 20:24 rmt -> /usr/sbin/rmt
drwxr-xr-x 4 root root   4.0K Jul 20 14:51 security
drwxr-xr-x 2 root root   4.0K Jul 20 14:53 selinux
-rw-r----- 1 root shadow  501 Jul 20 14:44 shadow
-rw-r--r-- 1 root root    116 Jul 20 14:43 shells
drwxr-xr-x 2 root root   4.0K Jul 20 14:57 skel
-rw-r--r-- 1 root root      0 Jul 20 14:43 subgid
-rw-r--r-- 1 root root      0 Jul 20 14:43 subuid
```

**有目的地使用颜色.**
例如，您可能想突出一些文字，让用户注意到它，或者用红色来表示错误。不要过度使用它--如果所有的东西都是不同的颜色，那么这个颜色就毫无意义，只会使它更难以阅读。

**如果您的程序不在终端中运行或用户请求禁用它，则禁用颜色。**
下面的情况需要禁用颜色:

- `stdout` 或 `stderr` 不是一个交互式终端 (是一个 TTY).
  最好单独检查——如果您将 `stdout` 传送到另一个程序，在 `stderr` 上获取颜色仍然很有用。
- 设置 `NO_COLOR` 环境变量.
- `TERM` 环境变量的值为 `dumb`.
- 用户传入了选项 `--no-color`.
- 您可能还想添加一个 `MYAPP_NO_COLOR` 环境变量，以防用户想要专门为您的程序禁用颜色。

_Further reading: [no-color.org](https://no-color.org/), [12 Factor CLI Apps](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46)_

**如果 `stdout` 不是交互式终端，则不要显示任何动画.**
这将阻止进度条在 CI 日志输出中变成圣诞树。

**使用符号和 emoji，它使事情变得更清晰.**
在能使事情更清晰的地方使用符号和表情符号。如果您需要让几件事情与众不同，吸引用户的注意力，或者只是增加一点特色，图片可能比文字更好。但要小心，很容易做得过头，使您的程序看起来杂乱无章或感觉像个玩具。

比如, [yubikey-agent](https://github.com/FiloSottile/yubikey-agent) 使用表情符号为输出添加结构，因此它不仅仅是一堵文本墙，还有一个 ❌ 来吸引您对重要信息的注意:

```shell-session
$ yubikey-agent -setup
🔐 The PIN is up to 8 numbers, letters, or symbols. Not just numbers!
❌ The key will be lost if the PIN and PUK are locked after 3 incorrect tries.

Choose a new PIN/PUK: 
Repeat the PIN/PUK: 

🧪 Retriculating splines …

✅ Done! This YubiKey is secured and ready to go.
🤏 When the YubiKey blinks, touch it to authorize the login.

🔑 Here's your new shiny SSH public key:
ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCEJ/
UwlHnUFXgENO3ifPZd8zoSKMxESxxot4tMgvfXjmRp5G3BGrAnonncE7Aj11pn3SSYgEcrrn2sMyLGpVS0=

💭 Remember: everything breaks, have a backup plan for when this YubiKey does.
```

**默认情况下，不要输出只有软件作者才能理解的信息.**
如果一个输出只是为了帮助您（开发者）理解您的软件在做什么，那么几乎可以肯定的是，它不应该以默认的方式显示给普通用户，而只是在 verbose 模式下。

邀请局外人和新加入您的项目的人提供可用性反馈。
他们会帮助您看到那些您离代码太近而没有注意到的重要问题。

**不要将 `stderr` 视为日志文件，至少默认情况下不要这样做。**
不要打印日志级别标签 (`ERR`, `WARN`, etc.) 或不相干的上下文信息，除非在 verbose 模式下。

**如果要输出大量文本，请使用分页 (e.g. `less`)。**
比如, `git diff` 默认做这件事。
使用分页可能很容易出错，所以要注意您的实现，不要让用户的体验变得更糟。
如果 `stdin` 或 `stdout` 不是一个交互式终端，您就不应该使用分页。

用于 `less` 的一套合理的选项是 `less -FIRX`。如果内容占满了一个屏幕，它就不翻页，搜索时忽略大小写，启用颜色和格式化，并且在 `less` 退出时把内容留在屏幕上。

在您的语言中，可能有一些库实现分页的方式比用管道输送到 `less` 更强大。
例如，Python 中的 [pypager](https://github.com/prompt-toolkit/pypager)。

### 错误 {#errors}

查阅文档的最常见的原因之一是为了修正错误。
如果您能把处理错误的方法整理进文档，那么这将为用户节省大量的时间。

**捕获错误并[为人们重写之](https://www.nngroup.com/articles/error-message-guidelines/)。**
如果您预计会有错误发生，那就捕获错误并重写错误信息，使之成为有用的信息。
把它想成是一次对话，用户做错了事，程序在引导他们走向正确的方向。
例如。"不能写到 file.txt。您可能需要通过运行 'chmod +w file.txt' 来使其可写。"

**信噪比至关重要。**
您产生的不相关的输出越多，用户就会花更多的时间来弄清楚他们做错了什么。
如果您的程序产生了多个相同类型的错误，考虑将它们分组在一个解释性的标题下，而不是打印许多看起来相似的行。

**考虑用户首先会看哪里。**
把最重要的信息放在输出的最后。
用户的视线会被红色的文字所吸引，所以要有意识地谨慎使用它。

**如果有一个意外的或无法解释的错误，那么提供调试和跟踪信息，以及关于如何提交错误的说明。**
也就是说，不要忘记信噪比：您不希望用他们不理解的信息来淹没用户。
考虑将调试日志写到一个文件中，而不是打印到终端。

**让提交错误报告变得毫不费力。**
您需要提供一个 URL，并让它预先填入尽可能多的信息。

### 参数与选项 {#arguments-and-flags}

术语:

- _Arguments_, 或 _args_, 是命令的位置参数.
  比如，您提供给 `cp` 的文件路径是 args.
  args 的顺序是很重要的：`cp foo bar` 的意思与 `cp bar foo` 是不同的。
- _Flags_ 是命名参数, 用连字符和单字母名称 (`-r`) 或双连字符和多字母名称 (`--recursive`) 表示.
  它们可能包含也可能不包含用户指定的值 (`--file foo.txt`, or `--file=foo.txt`).
  一般来说，flags 的顺序不会影响程序语义

**相比于 args 优先使用 flags。**
虽然这个可能要多打一些字，但是这会让接下来的工作更加清晰。
这也会使用户更加容易适应在未来输入方式的变化。
有些时候，在使用参数时，不太可能在不破坏现有行为或造成歧义的情况下增加新的输入。

_Citation: [12 Factor CLI Apps](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46)._

**所有的 flags 都应该有完整版本。**
比如，我们有 `-h` 和 `--help`。
flag 拥有完整的版本在脚本中是很有必要的，有时我们希望我们的脚本更具有描述性，这时完整版本的 flag 能够使您不必到处查找 flag 的含义。

_Citation: [GNU Coding Standards](https://www.gnu.org/prep/standards/html_node/Command_002dLine-Interfaces.html)._

**仅对常用的 flag 使用单字母 flag,** 尤其是在顶层使用子命令时。
这样您就不会 "污染 "您的 flag 的命名空间，避免让您对您将来添加的 flag 使用复杂的字母和大小写。

**对于针对多个文件的简单操作，指定多个 argument 是可行的。**
比如, `rm file1.txt file2.txt file3.txt`.
使用通配符同样可行: `rm *.txt`.

**如果被使用了多个不同种类的 argument ，那么用户或许操作错误了。**
除非这是一个常用的操作，在这种情况下，您应该提供一个更明确的描述。
比如, `cp <source> <destination>`.

_Citation: [12 Factor CLI Apps](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46)._

**如果有专门的名称标准的话，请使用标准的 flag 名称。**
如果另一个常用的命令使用了一个 flag 名称，最好是遵循这个现有的模式。
这样，用户就不必记住两个不同的选项（以及它适用于哪个命令），用户甚至可以猜测一个选项，而不必看帮助文本。

下面是常用的选项:

- `-a`, `--all`: All.
  For example, `ps`, `fetchmail`.
- `-d`, `--debug`: Show debugging output.
- `-f`, `--force`: Force.
  For example, `rm -f` will force the removal of files, even if it thinks it does not have permission to do it.
  This is also useful for commands which are doing something destructive that usually require user confirmation, but you want to force it to do that destructive action in a script.
- `--json`: Display JSON output.
  See the [output](#output) section.
- `-h`, `--help`: Help.
  This should only mean help.
  See the [help](#help) section.
- `--no-input`: See the [interactivity](#interactivity) section.
- `-o`, `--output`: Output file.
  For example, `sort`, `gcc`.
- `-p`, `--port`: Port.
  For example, `psql`, `ssh`.
- `-q`, `--quiet`: Quiet.
  Display less output.
  This is particularly useful when displaying output for humans that you might want to hide when running in a script.
- `-u`, `--user`: User.
  For example, `ps`, `ssh`.
- `--version`: Version.
- `-v`: This can often mean either verbose or version.
  You might want to use `-d` for verbose and this for version, or for nothing to avoid confusion.

**让默认值成为大多数用户的正确选择。**
可配置化是不错的，但大多数用户不会找到正确的 flags 并记得一直使用它（或别名）。
如果它不是默认的，您就会使您的大多数用户的体验变差。

比如，`ls` 有简明的默认输出以优化脚本和其他历史问题, 但如果是今天的设计，它可能会默认是 `ls -lhFGT`.

**向用户提供 prompt。**
如果用户没有传递 argument 或 flag，那就进行 prompt。
(See also: [交互性](#interactivity))

**不再 _需要_ prompt.**
向用户一直提供一种传递 flags 和 arguments 的输入方式。
如果 `stdin` 不是一个交互式终端，跳过 prompting 并只需要 flags/args。

**在做任何危险的操作之前进行确认。**
如果运行在交互式环境，一个常见的做法是提示用户键入 `y` or `yes`, 或者要求用户传入 `-f` or `--force`.

“危险性” 是一个主观性术语, 存在不同等级的危险:

- **Mild:** 一个小的、局部的改变，如删除一个文件。
  您可能想提示确认，也可能不想。
  例如，如果用户明确地运行一个叫做 "删除 "的命令，您可能不需要询问。
- **Moderate:** 一个较大的本地变化，如删除一个目录，一个远程变化，如删除某种资源，或一个复杂的批量修改，不容易被撤销。
  你通常想在这里提示确认。
  考虑给用户一个 "dry run "操作的方法，这样他们就可以在提交之前看到会发生什么。
- **Severe:** 删除一些复杂的东西，如整个远程应用程序或服务器。
  你不只是想在这里提示确认--你想让它很难被意外确认。
  考虑要求他们输入一些非琐碎的东西，如他们要删除的东西的名称。
  让他们传递一个 flag，如 `--confirm="name-of-thing"` ，这样就可以用脚本编写了。

考虑是否有不明显的方式意外地破坏事物。
例如，想象一下这样一种情况：将配置文件中的一个数字从 10 改为 1，意味着 9 个东西将被隐式删除--这应该被认为是一种严重的风险，而且应该很难意外地做到。

**如果输入或输出是一个文件, 支持使用 `-` 来从 `stdin` 中读或写出到 `stdout`.**
这让另一个命令的输出成为你的命令的输入，反之亦然，从而不需要使用临时文件。
比如，`tar` 可以从 `stdin` 中提取文件：

```
$ curl https://example.com/something.tar.gz | tar xvf -
```

**如果一个 flag 可以接受一个可选的值，那么允许使用一个特殊的词，如 "none."。**
例如，`ssh -F` 需要一个可选的 `ssh_config` 文件的文件名，而用 `ssh -F none` 运行 SSH，则无需配置文件。不要只使用一个空白值--这可能会使参数是 flag values 还是 arguments 变得模糊不清。

**如果可以，尽量让 arguments, flags, subcommands 顺序无关**
很多 CLI，特别是那些含有子命令的 CLI，各种参数的位置顺序存在一定的潜规则。
例如，一个命令含有一个 `--foo` flag，只有当您把它放在子命令之前时才会起作用：

```
mycmd --foo=1 subcmd
works

$ mycmd subcmd --foo=1
unknown flag: --foo
```

这可能会让用户感到非常困惑--特别是用户常常会复制上次执行的命令，在最后面加上另一个 option，然后再次执行。
尽量让两种形式等同，尽管你可能会遇到参数分析器的限制。

**允许通过文件传入敏感的参数值。**
假设您的命令通过 `--password` 参数获取敏感信息。
一个 raw `--password` 参数会将会将秘密泄露到 `ps` 输出中，也可能泄露到 shell history 中。
考虑只允许通过文件输入秘文，例如使用一个 `--password-file` 参数。
一个 `--password-file` 参数允许在各种情况下谨慎地传入敏感信息。

(在 Bash 中可以通过使用 `--password $(< password.txt)` 将文件的内容传递到一个参数中。
不幸的是，并不是每个命令环境都可以使用神奇的 shell 替换。
例如，`systemd` 服务，`exec` 系统调用，以及一些 `Dockerfile` 命令形式不支持大多数 shell 中的替换。
更重要的是，这种方法同样存在安全问题，即把文件内容泄露到 `ps` 的输出等地方。
最好避免这种做法)。

### 交互性 {#interactivity}

**Only use prompts or interactive elements if `stdin` is an interactive terminal (a TTY).**
This is a pretty reliable way to tell whether you’re piping data into a command or whether it's being run in a script, in which case a prompt won’t work and you should throw an error telling the user what flag to pass.

**If `--no-input` is passed, don’t prompt or do anything interactive.**
This allows users an explicit way to disable all prompts in commands.
If the command requires input, fail and tell the user how to pass the information as a flag.

**If you’re prompting for a password, don’t print it as the user types.**
This is done by turning off echo in the terminal.
Your language should have helpers for this.

**Let the user escape.**
Make it clear how to get out.
(Don’t do what vim does.)
If your program hangs on network I/O etc, always make Ctrl-C still work.
If it’s a wrapper around program execution where Ctrl-C can’t quit (SSH, tmux, telnet, etc), make it clear how to do that.
For example, SSH allows escape sequences with the `~` escape character.

### 子命令

If you’ve got a tool that’s sufficiently complex, you can reduce its complexity by making a set of subcommands.
If you have several tools that are very closely related, you can make them easier to use and discover by combining them into a single command (for example, RCS vs. Git).

They’re useful for sharing stuff—global flags, help text, configuration, storage mechanisms.

**Be consistent across subcommands.**
Use the same flag names for the same things, have similar output formatting, etc. 

**Use consistent names for multiple levels of subcommand.**
If a complex piece of software has lots of objects and operations that can be performed on those objects, it is a common pattern to use two levels of subcommand for this, where one is a noun and one is a verb.
For example, `docker container create`.
Be consistent with the verbs you use across different types of objects.

Either `noun verb` or `verb noun` ordering works, but `noun verb` seems to be more common.

_Further reading: [User experience, CLIs, and breaking the world, by John Starich](https://uxdesign.cc/user-experience-clis-and-breaking-the-world-baed8709244f)._

**Don’t have ambiguous or similarly-named commands.**
For example, having two subcommands called “update” and “upgrade” is quite confusing.
You might want to use different words, or disambiguate with extra words.

### 鲁棒性 {#robustness-guidelines}

**Validate user input.**
Everywhere your program accepts data from the user, it will eventually be given bad data.
Check early and bail out before anything bad happens, and [make the errors understandable](#errors).

**Responsive is more important than fast.**
Print something to the user in <100ms.
If you’re making a network request, print something before you do it so it doesn’t hang and look broken.

**Show progress if something takes a long time.**
If your program displays no output for a while, it will look broken.
A good spinner or progress indicator can make a program appear to be faster than it is.

Ubuntu 20.04 has a nice progress bar that sticks to the bottom of the terminal.

<!-- (TK reproduce this as a code block or animated SVG) -->

If the progress bar gets stuck in one place for a long time, the user won’t know if stuff is still happening or if the program’s crashed.
It’s good to show estimated time remaining, or even just have an animated component, to reassure them that you’re still working on it.

There are many good libraries for generating progress bars.
For example, [tqdm](https://github.com/tqdm/tqdm) for Python, [schollz/progressbar](https://github.com/schollz/progressbar) for Go, and [node-progress](https://github.com/visionmedia/node-progress) for Node.js.

**Do stuff in parallel where you can, but be thoughtful about it.**
It’s already difficult to report progress in the shell; doing it for parallel processes is ten times harder.
Make sure it’s robust, and that the output isn’t confusingly interleaved.
If you can use a library, do so—this is code you don’t want to write yourself.
Libraries like [tqdm](https://github.com/tqdm/tqdm) for Python and [schollz/progressbar](https://github.com/schollz/progressbar) for Go support multiple progress bars natively.

The upside is that it can be a huge usability gain.
For example, `docker pull`’s multiple progress bars offer crucial insight into what’s going on.

<!-- (TK docker pull animation) -->

One thing to be aware of: hiding logs behind progress bars when things go _well_ makes it much easier for the user to understand what’s going on, but if there is an error, make sure you print out the logs.
Otherwise, it will be very hard to debug.

**Make things time out.**
Allow network timeouts to be configured, and have a reasonable default so it doesn’t hang forever.

**Make it idempotent.**
If the program fails for some transient reason (e.g. the internet connection went down), you should be able to hit `<up>` and `<enter>` and it should pick up from where it left off.

**Make it crash-only.**
This is the next step up from idempotence.
If you can avoid needing to do any cleanup after operations, or you can defer that cleanup to the next run, your program can exit immediately on failure or interruption.
This makes it both more robust and more responsive.

_Citation: [Crash-only software: More than meets the eye](https://lwn.net/Articles/191059/)._

**People are going to misuse your program.**
Be prepared for that.
They will wrap it in scripts, use it on bad internet connections, run many instances of it at once, and use it in environments you haven’t tested in, with quirks you didn’t anticipate.
(Did you know macOS filesystems are case-insensitive but also case-preserving?)

### 前瞻性 {#future-proofing}

In software of any kind, it’s crucial that interfaces don’t change without a lengthy and well-documented deprecation process.
Subcommands, arguments, flags, configuration files, environment variables: these are all interfaces, and you’re committing to keeping them working.
([Semantic versioning](https://semver.org/) can only excuse so much change; if you’re putting out a major version bump every month, it’s meaningless.)

**Keep changes additive where you can.**
Rather than modify the behavior of a flag in a backwards-incompatible way, maybe you can add a new flag—as long as it doesn’t bloat the interface too much.
(See also: [Prefer flags to args](#arguments-and-flags).)

**Warn before you make a non-additive change.**
Eventually, you’ll find that you can’t avoid breaking an interface.
Before you do, forewarn your users in the program itself: when they pass the flag you’re looking to deprecate, tell them it’s going to change soon.
Make sure there’s a way they can modify their usage today to make it future-proof, and tell them how to do it.

If possible, you should detect when they’ve changed their usage and not show the warning any more: now they won’t notice a thing when you finally roll out the change.

**Changing output for humans is usually OK.**
The only way to make an interface easy to use is to iterate on it, and if the output is considered an interface, then you can’t iterate on it.
Encourage your users to use `--plain` or `--json` in scripts to keep output stable (see [Output](#output)).

**Don’t have a catch-all subcommand.**
If you have a subcommand that’s likely to be the most-used one, you might be tempted to let people omit it entirely for brevity’s sake.
For example, say you have a `run` command that wraps an arbitrary shell command:

    $ mycmd run echo "hello world"

You could make it so that if the first argument to `mycmd` isn’t the name of an existing subcommand, you assume the user means `run`, so they can just type this:

    $ mycmd echo "hello world"

This has a serious drawback, though: now you can never add a subcommand named `echo`—or _anything at all_—without risking breaking existing usages.
If there’s a script out there that uses `mycmd echo`, it will do something entirely different after that user upgrades to the new version of your tool.

**Don’t allow arbitrary abbreviations of subcommands.**
For example, say your command has an `install` subcommand.
When you added it, you wanted to save users some typing, so you allowed them to type any non-ambiguous prefix, like `mycmd ins`, or even just `mycmd i`, and have it be an alias for `mycmd install`.
Now you’re stuck: you can’t add any more commands beginning with `i`, because there are scripts out there that assume `i` means `install`.

There’s nothing wrong with aliases—saving on typing is good—but they should be explicit and remain stable.

**Don’t create a “time bomb.”**
Imagine it’s 20 years from now.
Will your command still run the same as it does today, or will it stop working because some external dependency on the internet has changed or is no longer maintained?
The server most likely to not exist in 20 years is the one that you are maintaining right now.
(But don’t build in a blocking call to Google Analytics either.)

### 信号和控制字符 {#signals}

**If a user hits Ctrl-C (the INT signal), exit as soon as possible.**
Say something immediately, before you start clean-up.
Add a timeout to any clean-up code so it can’t hang forever.

**If a user hits Ctrl-C during clean-up operations that might take a long time, skip them.**
Tell the user what will happen when they hit Ctrl-C again, in case it is a destructive action.

For example, when quitting Docker Compose, you can hit Ctrl-C a second time to force your containers to stop immediately instead of shutting them down gracefully.

```
$  docker-compose up
…
^CGracefully stopping... (press Ctrl+C again to force)
```

Your program should expect to be started in a situation where clean-up has not been run.
(See [Crash-only software: More than meets the eye](https://lwn.net/Articles/191059/).)

### 配置 {#configuration}

Command-line tools have lots of different types of configuration, and lots of different ways to supply it (flags, environment variables, project-level config files).
The best way to supply each piece of configuration depends on a few factors, chief among them _specificity_, _stability_ and _complexity_.

Configuration generally falls into a few categories:

1.  Likely to vary from one invocation of the command to the next.

    Examples:

    - Setting the level of debugging output
    - Enabling a safe mode or dry run of a program

    Recommendation: **Use [flags](#arguments-and-flags).**
    [Environment variables](#environment-variables) may or may not be useful as well.

2.  Generally stable from one invocation to the next, but not always.
    Might vary between projects.
    Definitely varies between different users working on the same project.

    This type of configuration is often specific to an individual computer.

    Examples:

    - Providing a non-default path to items needed for a program to start
    - Specifying how or whether color should appear in output
    - Specifying an HTTP proxy server to route all requests through

    Recommendation: **Use [flags](#arguments-and-flags) and probably [environment variables](#environment-variables) too.**
    Users may want to set the variables in their shell profile so they apply globally, or in `.env` for a particular project.

    If this configuration is sufficiently complex, it may warrant a configuration file of its own, but environment variables are usually good enough.

3.  Stable within a project, for all users.

    This is the type of configuration that belongs in version control.
    Files like `Makefile`, `package.json` and `docker-compose.yml` are all examples of this.

    Recommendation: **Use a command-specific, version-controlled file.**

**Follow the XDG-spec.**
In 2010 the X Desktop Group, now [freedesktop.org](https://freedesktop.org), developed a specification for the location of base directories where config files may be located.
One goal was to limit the proliferation of dotfiles in a user’s home directory by supporting a general-purpose `~/.config` folder.
The XDG Base Directory Specification ([full spec](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html), [summary](https://wiki.archlinux.org/index.php/XDG_Base_Directory#Specification)) is supported by yarn, fish, wireshark, emacs, neovim, tmux, and many other projects you know and love.

**If you automatically modify configuration that is not your program’s, ask the user for consent and tell them exactly what you’re doing.**
Prefer creating a new config file (e.g. `/etc/cron.d/myapp`) rather than appending to an existing config file (e.g. `/etc/crontab`).
If you have to append or modify to a system-wide config file, use a dated comment in that file to delineate your additions.

**Apply configuration parameters in order of precedence.**
Here is the precedence for config parameters, from highest to lowest:

- Flags
- The running shell’s environment variables
- Project-level configuration (eg. `.env`)
- User-level configuration
- System wide configuration

### 环境变量 {#environment-variables}

**Environment variables are for behavior that _varies with the context_ in which a command is run.**
The “environment” of an environment variable is the terminal session—the context in which the command is running.
So, an env var might change each time a command runs, or between terminal sessions on one machine, or between instantiations of one project across several machines.

Environment variables may duplicate the functionality of flags or configuration parameters, or they may be distinct from those things.
See [Configuration](#configuration) for a breakdown of common types of configuration and recommendations on when environment variables are most appropriate.

**For maximum portability, environment variable names must only contain uppercase letters, numbers, and underscores (and mustn't start with a number).**
Which means `O_O` and `OWO` are the only emoticons that are also valid environment variable names.

**Aim for single-line environment variable values.**
While multi-line values are possible, they create usability issues with the `env` command.

**Avoid commandeering widely used names.**
Here’s a [list of POSIX standard env vars](https://pubs.opengroup.org/onlinepubs/009695399/basedefs/xbd_chap08.html).

**Check general-purpose environment variables for configuration values when possible:**

- `NO_COLOR`, to disable color (see [Output](#output)).
- `DEBUG`, to enable more verbose output.
- `EDITOR`, if you need to prompt the user to edit a file or input more than a single line.
- `HTTP_PROXY`, `HTTPS_PROXY`, `ALL_PROXY` and `NO_PROXY`, if you’re going to perform network operations.
  (The HTTP library you’re using might already check for these.)
- `SHELL`, if you need to open up an interactive session of the user's preferred shell.
  (If you need to execute a shell script, use a specific interpreter like `/bin/sh`)
- `TERM`, `TERMINFO` and `TERMCAP`, if you’re going to use terminal-specific escape sequences.
- `TMPDIR`, if you’re going to create temporary files.
- `HOME`, for locating configuration files.
- `PAGER`, if you want to automatically page output.
- `LINES` and `COLUMNS`, for output that’s dependent on screen size (e.g. tables).

**Read environment variables from `.env` where appropriate.**
If a command defines environment variables that are unlikely to change as long as the user is working in a particular directory, then it should also read them from a local `.env` file so users can configure it differently for different projects without having to specify them every time.
Many languages have libraries for reading `.env` files ([Rust](https://crates.io/crates/dotenv), [Node](https://www.npmjs.com/package/dotenv), [Ruby](https://github.com/bkeepers/dotenv)).

**Don’t use `.env` as a substitute for a proper [configuration file](#configuration).**
`.env` files have a lot of limitations:

- A `.env` file is not commonly stored in source control
- (Therefore, any configuration stored in it has no history)
- It has only one data type: string
- It lends itself to being poorly organized
- It makes encoding issues easy to introduce
- It often contains sensitive credentials & key material that would be better stored more securely

If it seems like these limitations will hamper usability or security, then a dedicated config file might be more appropriate.

### 命名 {#naming}

The name of your program is particularly important on the CLI: your users will be typing it all the time, and it needs to be easy to remember and type.

**Make it a simple, memorable word.**
But not too generic, or you’ll step on the toes of other commands and confuse users.
For example, both ImageMagick and Windows used the command `convert`.

**Use only lowercase letters, and dashes if you really need to.**
`curl` is a good name, `DownloadURL` is not.

**Keep it short.**
Users will be typing it all the time.
Don’t make it _too_ short: the very shortest commands are best reserved for the common utilities used all the time, such as `cd`, `ls`, `ps`.

**Make it easy to type.**
Some words flow across the QWERTY keyboard much more easily than others, and it’s not just about brevity.
`plum` may be short but it’s an awkward, angular dance.
`apple` trips you up with the double letter.
`orange` is longer than both, but flows much better.

_Further reading: [The Poetics of CLI Command Names](https://smallstep.com/blog/the-poetics-of-cli-command-names/)_

### 发布 {#distribution}

**If possible, distribute as a single binary.**
If your language doesn’t compile to binary executables as standard, see if it has something like [PyInstaller](https://www.pyinstaller.org/).
If you really can’t distribute as a single binary, use the platform’s native package installer so you aren’t scattering things on disk that can’t easily be removed.
Tread lightly on the user’s computer.

If you’re making a language-specific tool, such as a code linter, then this rule doesn’t apply—it’s safe to assume the user has an interpreter for that language installed on their computer.

**Make it easy to uninstall.**
If it needs instructions, put them at the bottom of the install instructions—one of the most common times people want to uninstall software is right after installing it.

### 分析数据 {#analytics}

Usage metrics can be helpful to understand how users are using your program, how to make it better, and where to focus effort.
But, unlike websites, users of the command-line expect to be in control of their environment, and it is surprising when programs do things in the background without telling them.

**Do not phone home usage or crash data without consent.**
Users will find out, and they will be angry.
Be very explicit about what you collect, why you collect it, how anonymous it is and how you go about anonymizing it, and how long you retain it for.

Ideally, ask users whether they want to contribute data (“opt-in”).
If you choose to do it by default (“opt-out”), then clearly tell users about it on your website or first run, and make it easy to disable.

Examples of projects that collect usage statistics:

- Angular.js [collects detailed analytics using Google Analytics](https://angular.io/analytics), in the name of feature prioritization.
  You have to explicitly opt in.
  You can change the tracking ID to point to your own Google Analytics property if you want to track Angular usage inside your organization.
- Homebrew sends metrics to Google Analytics and has [a nice FAQ](https://docs.brew.sh/Analytics) detailing their practices.
- Next.js [collects anonymized usage statistics](https://nextjs.org/telemetry) and is enabled by default.

**Consider alternatives to collecting analytics.**

- Instrument your web docs.
  If you want to know how people are using your CLI tool, make a set of docs around the use cases you’d like to understand best, and see how they perform over time.
  Look at what people search for within your docs.
- Instrument your downloads.
  This can be a rough metric to understand usage and what operating systems your users are running.
- Talk to your users.
  Reach out and ask people how they’re using your tool.
  Encourage feedback and feature requests in your docs and repos, and try to draw out more context from those who submit feedback.

_Further reading: [Open Source Metrics](https://opensource.guide/metrics/)_

## 深入阅读

- [The Unix Programming Environment](https://en.wikipedia.org/wiki/The_Unix_Programming_Environment), Brian W. Kernighan and Rob Pike
- [POSIX Utility Conventions](https://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap12.html)
- [Program Behavior for All Programs](https://www.gnu.org/prep/standards/html_node/Program-Behavior.html), GNU Coding Standards
- [12 Factor CLI Apps](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46), Jeff Dickey
- [CLI Style Guide](https://devcenter.heroku.com/articles/cli-style-guide), Heroku

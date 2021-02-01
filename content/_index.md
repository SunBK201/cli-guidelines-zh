# 命令行界面设计指南

这是一个[开源指南](https://github.com/SunBK201/cli-guidelines-zh)，可帮助您编写更好的命令行程序，采用经典的UNIX哲学并对其进行优化，以适应现代生产环境的需要。

## 译者 {#authors}

**SunBK201** 

原项目是[cli-guidelines](https://github.com/cli-guidelines/cli-guidelines)，译者对其进行了汉化翻译，以供英文不好的朋友阅读。

[@SunBK201](https://github.com/SunBK201)

<iframe class="github-button" src="https://ghbtns.com/github-btn.html?user=SunBK201&repo=cli-guidelines-zh&type=star&count=true&size=large" frameborder="0" scrolling="0" width="170" height="30" title="GitHub"></iframe>

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

在20世纪80年代，如果你想要一台个人计算机为你做一些事情，在面对 `C:\>` 或 `~$` 时，你需要知道该输入什么。
帮助是以厚重的螺旋装订手册提供的，
错误消息是不透明的，
没有Stack Overflow可以拯救你。
但是，如果你有幸能够访问Internet，那么你可以从Usenet（一个早期的Internet社区）获得帮助，那里面满是和你一样沮丧的人。
他们可以帮助你解决问题，或者至少提供一些精神支持。

40年后，每个人都变得能够使用计算机，但这通常是在牺牲以底层终端用户控制为代价实现的。在许多设备上，根本没有命令行访问方式，一部分原因是这与“围墙花园”和应用商店公司的利益相悖。

今天的大多数人都不知道命令行是什么，更不用说他们为什么要费心使用它了。正如计算机先驱Alan Kay在[2017年的一次采访](https://www.fastcompany.com/40435064/what-alan-kay-thinks-about-the-iphone-and-technology-now)中所说，“因为人们不理解计算机是什么，他们以为自己在iPhone里就有了计算机，这种错觉就像 '吉他英雄' 与真吉他一样糟糕”。

Kay的“真吉他”不完全是CLI。他谈论的是计算机编程的方法，它提供CLI的功能，超越了用文本文件编写软件。Kay的学生们认为，我们需要打破我们已经生活了几十年的基于文本的局部最大值。

想象未来我们用完全不同的方式来编写计算机程序是一件令人兴奋的事情。即使在今天，电子表格仍然是迄今为止最流行的编程语言，而无代码运动正在迅速发展，因为它试图取代一些对有才华的程序员的强烈需求

然而，尽管命令行有其陈旧的、数十年之久的约束和令人费解的怪异，但它依旧是计算机中最 _万能_ 的一角。
它让你拉开计算机真正的面纱，看看里面到底发生了什么，并使你创造性地以GUI无法承受的复杂和深度与机器进行交互。
任何想学习它的人都可以在几乎所有的笔记本电脑上使用它。
它可以交互使用，也可以自动化使用。
此外，它的变化速度不如系统其他部分快。
其创造价值源于稳定性。

所以，当我们仍然拥有它的时候，我们应该尽量最大化它的效用和可访问性。

从很早之前开始，我们的计算机编程方式发生了很大的变化。
过去的命令行是 _机器优先_ 的: 和脚本平台上的REPL差不多。
但是，随着通用解释语言的蓬勃发展，shell脚本的作用已经缩小。
今天的命令行是 _以人为本_ 的: 一个基于文本的UI，提供对各种工具、系统和平台的访问。
在过去，编辑器是在终端里的，今天终端通常是编辑器的一个功能。
类似`git`的多工具命令也在大量涌现。
命令中的命令，以及执行整个工作流而不是原子功能的高级命令。

受传统UNIX原则的启发，受令人愉快和可访问的CLI环境的兴趣驱动，以及在我们作为程序员的经验的指导下，我们决定是时候重温构建命令行程序的最佳实践和设计原则了。

命令行万岁！

## 介绍 {#introduction}

本文档涵盖了高层次的设计理念和具体的指导方针。
它在指导方针上更加偏重，因为作为实践者，我们的原则是不需要进行太多的高谈阔论。
我们相信要通过例子学习，因此我们也准备了不少例子以供大家学习。

这篇指南不会包含类似emacs和vim那样全屏终端程序。
全屏程序是小众项目，很少有人能设计出这样的项目。

一般来说，本指南也不涉及编程语言和工具。

本指南适用于谁？
- 如果您正在创建一个CLI程序，并且您正在为它的UI设计寻找原则和具体的最佳实践，那么本指南就是为您准备的。
- 如果您是专业的 “CLI UI设计师” ，那就太好了，我们很乐意向您学习。
- 如果您想避免与40年来的CLI设计惯例相悖的错误，本指南非常适合您。
- 如果您想通过程序的良好设计和有用的帮助使人们满意，那么本指南绝对适合您。
- 如果您正在创建一个GUI程序，这篇指南不适合你，但是如果你决定阅读它，你可能会学到一些GUI的错误模式。
- 如果您正在设计Minecraft的沉浸式全屏CLI端口，则本指南不适合您。
  (但我们已经等不及要看了!)

## 原则 {#philosophy}

这些是我们认为优秀的CLI设计的基本原则。

### 以人为本设计 {#human-first-design}

传统上，UNIX命令是在假定它们将主要由其他程序使用的情况下编写的。
与图形应用程序相比，它们与编程语言中的函数有更多的共同之处。

如今，尽管许多CLI程序主要(甚至专门)由人类使用，但它们的许多交互设计仍然承载着过去的包袱。
现在是时候摆脱这些包袱了: 如果一个命令主要是由人类使用的，那么它应该首先为人类设计。

### 简单部件协同工作 {#simple-parts-that-work-together}

[最初的UNIX哲学](https://en.wikipedia.org/wiki/Unix_philosophy)中一个核心原则是具有干净接口的简单程序可以组合起来构建更大的系统。
与往这些程序中添加越来越多的特性不同，您制作的程序足够模块化，可以根据需要进行重构。

在过去，管道和Shell脚本在将程序组合在一起的过程中起着至关重要的作用。
随着通用解释语言的兴起，它们的作用可能已减弱，但它们的确没有消失。
更重要的是，以CI/CD，业务流程和配置管理形式的大型自动化蓬勃发展。
使程序具有可组合性和以往一样重要。

幸运的是，UNIX环境中长期建立的约定(专为这一目的而设计)至今仍对我们有帮助。
标准的输入/输出/错误，信号，退出代码和其他机制可确保不同的程序很好地连接在一起。
普通的、基于行的文本很容易在命令之间进行管道连接。
JSON是一项较新的发明，它在需要时为我们提供了更多结构，并使我们可以更轻松地将命令行工具与Web集成。

无论您要构建什么软件，都可以绝对确定人们会以您意想不到的方式使用它。
您的软件 _将_ 成为一个更大系统中的一部分，您唯一的选择是它是否是一个行为良好的部分。

最重要的是，针对可组合性的设计不必与以人为本的设计相矛盾。
本文中的大部分建议都是关于如何同时实现这两者的。

### 跨程序的一致性 {#consistency-across-programs}

终端的约定已牢牢扎根于我们的手指。
我们在前期必须支付成本来学习命令行语法、标志、环境变量等等，但只要程序是一致的，它就会在长期的效率方面得到回报。

CLI应尽可能地遵循已经存在地模式。
这就是使CLI直观易懂的原因。也就是使用户高效的原因。

话虽如此，有时一致性与易用性也会产生冲突。
例如，许多历史悠久的UNIX命令默认情况下不会输出太多信息，这可能会给那些不太熟悉命令行的人造成混乱或担忧。

当遵循惯例会损害程序的可用性时，可能是时候打破它了，但这样的决定应该谨慎做出。

### 输出适中 {#saying-just-enough}

终端是一个纯信息的世界。
你可以认为信息是一个界面，就像任何界面一样，经常有太多或太少的信息。

当命令挂起几分钟时，由于输出的信息太少了，用户开始怀疑它是否坏了。
当命令输出一页又一页的调试信息时，由于输出的信息太多了，反而淹没了在松散碎片中真正重要的部分。
最终的结果是相同的: 缺乏清晰度，让用户感到困惑和恼火。

这种平衡很难做到，但如果软件要为用户提供服务，这绝对是至关重要的。

### 易发现性 {#ease-of-discovery}

当涉及到功能的可发现性时，GUI占了上风。
你能做的一切都显示在你面前的屏幕上，因此您无需学习任何内容就可以找到所需的功能，甚至可以发现自己不知道的功能。

假设命令行界面与此相反，您必须记住如何做所有的事情。
最初的 [Macintosh人机界面指南](https://archive.org/details/applehumaninterf00appl)，发布于1987年，推荐使用 “看见并指向（而不是记住并键入）” 方式，就好像您只能选择其中一种一样。

这些事情不必相互排斥。
使用命令行的效率来自于记住命令，但是没有理由命令不能帮助您学习和记住命令。

可发现的CLI包含全面的帮助文本，提供许多示例，建议下一步要运行什么命令，发生错误时应采取的措施。
可以从GUI窃取很多想法，以使CLI易于学习和使用，即使对于高级用户也是如此。

_引用: The Design of Everyday Things (Don Norman)，Macintosh Human Interface Guidelines_

### 对话是常态 {#conversation-as-the-norm}

GUI设计，尤其是在早期，大量使用了 _比喻_: 桌面、文件、文件夹、回收站。
这是很有道理的，因为计算机仍然试图使自己更加合理。
比喻的易于实现是GUI相对于CLI的巨大优势之一。
然而，具有讽刺意味的是，CLI一直体现了一个偶然的比喻: 它是一场对话。

除了最简单的命令之外，运行一个程序通常涉及不止一次的调用。
通常，这是因为很难在第一次就正确使用它：用户键入命令，遇到错误，更改命令，遇到其他错误等等，直到可以正常使用。
通过反复失败来学习的这种模式就像用户与程序进行的对话。

不过，试错法并不是对话互动的唯一方式。
此外还有：

- 运行一个命令来设置工具，然后学习运行哪些命令才能真正开始使用它。
- 运行几个命令来设置一个操作，然后运行最后一个命令（比如 多个 `git add`，后面跟着一个 `git commit`）。
- 探索一个系统，例如用许多 `cd` 和 `ls` 来了解目录结构，或使用 `git log` 和 `git show` 来查看文件的历史。
- 在真正运行一个复杂的操作之前，先进行一次演习。

承认命令行交互的对话性意味着您可以将相关技术应用到它的设计中。
当用户输入无效时，您可以提出纠正建议; 当用户经历多个步骤的过程时，您可以明确中间状态; 您可以在用户做一些骚操作之前向他们确认一切进行正常。

用户正在与您的软件进行对话，不管您是否有意为之。
在最坏的情况下，这是一场充满敌意的谈话，让他们感到愚蠢和怨恨。
在最好的情况下，这是一种愉快的交流，可以加速他们获得新知识和成就感的过程。

_深入阅读: [The Anti-Mac User Interface (Don Gentner and Jakob Nielsen)](https://www.nngroup.com/articles/anti-mac-interface/)_

### 鲁棒性 {#robustness-principle}

鲁棒性既是一种客观属性，也是一种主观属性。
当然，软件 _应该_ 是健壮的: 异常的输入应该被妥善处理，操作应该在可能的情况下是幂等的，等等。
但它也应该 _感觉_ 是健壮的。

你想让你的软件在感觉上不会崩溃。
你希望它能给人一种即时响应的感觉，就好像它是一个巨大的机械机器，而不是一个脆弱的塑料“软开关”。

主观的鲁棒性需要关注细节，并认真思考可能出错的地方。
这里有许多小细节: 让用户知道当前正在发生什么，解释常见错误的含义，不打印可怕的堆栈跟踪。

通常，鲁棒性也可以来自简单性。
许多特殊情况和复杂的代码往往会使程序变得脆弱。

### 同感共情 {#empathy}

命令行工具是程序员的创作工具，所以使用起来应该很愉快。
这并不意味着要把它们变成一款电子游戏，或者使用大量的表情符号（尽管表情符号本身并没有什么错 😉）。
这意味着要让用户觉得你站在他们一边，你希望他们成功，你已经仔细考虑过他们的问题以及如何解决问题。

虽然我们希望遵循我们的建议会给你带来一些帮助，但你所能采取的行动并不能确保他们有这种感觉。
取悦用户意味着每一次都 _超过他们的期望_，这始于同感共情。

### 混乱 {#chaos}

这个世界的终端一团糟，
前后矛盾无处不在，拖慢了我们的脚步，让我们反思自己。

然而，不可否认的是，这种混乱一直是力量的源泉。
终端，就像一般的UNIX下的计算环境一样，对您可以构建的内容设置很少的限制。
在这个领域里，各种各样的发明都如雨后春笋般涌现。

具有讽刺意味的是，本文档劝告您遵循现有模式，并提出与数十年来命令行传统相矛盾的建议。
我们和其他人一样对违反规则感到内疚。

也许您也该打破规矩了。
有明确目的要这样做。

> “当一个标准明显损害了生产力或用户满意度时，那么是时候放弃这个标准了” — Jef Raskin, [The Humane Interface](https://en.wikipedia.org/wiki/The_Humane_Interface)

## 指南 {#guidelines}

这是一组可以使您的命令行程序进一步优化的特定操作。

第一部分包含您需要遵循的基本内容。
如果这一部分犯错，您的程序会很难用。

其余的部分也都值得去学习。
如果你有时间和精力去添加这些东西，你的程序会比其他一般的程序好很多。

如果你不打算花太多精力在程序的设计上，则不必：只需遵循这些规则，您的程序就可能很好。 
另一方面，如果您考虑过并确定以下规则对您的程序是错误的，那也是可以的。
(没有一个权威会因为没有遵循任意规则而拒绝你的程序。)

另外，这些规则不是一成不变的。
如果你有充分的理由不同意这些规则，您可以[提出建议](https://github.com/cli-guidelines/cli-guidelines).

### 基础 {#the-basics}

这里有一些您需要遵守的基本规则。
弄错这些，您的程序要么使用体验很差，要么就会彻底崩溃。

**可以的话，使用命令行参数解析库.**
命令行参数解析库要么是语言内置的，要么是优秀的第三方解析库。
这些解析库通常会以一种合理的方式处理参数、解析标记、提供帮助文本，甚至拼写建议。

以下是常见的一些解析库：
* Go: [Cobra](https://github.com/spf13/cobra), [cli](https://github.com/urfave/cli)
* Java: [picocli](https://picocli.info/)
* Node: [oclif](https://oclif.io/)
* Python: [Click](https://click.palletsprojects.com/), [Typer](https://github.com/tiangolo/typer)
* Ruby: [TTY](https://ttytoolkit.org/)
* Rust: [clap](https://clap.rs/), [structopt](https://github.com/TeXitoi/structopt)
* PHP: [console](https://github.com/symfony/console)

**成功时返回零退出码，失败时返回非零退出码.**
退出码是描述一个程序执行成功还是失败的方式，因此您应该正确地报告这一点。
此外，将非零的退出码映射到最重要的失败模式。

**将 output 发送到 `stdout`.**
您的命令的主要的 output 应该是 `stdout`.
任何机器可读的内容也应该转到 `stdout`—这是管道默认发送的地方。

**将 messaging 发送到 `stderr`.**
日志消息，错误等都应发送到 `stderr`.
这意味着当命令通过管道传输时，这些消息将显示给用户，而不是馈入下一个命令。

### 帮助 {#help}

**当程序运行没有任何启动选项, 或者用户输入 `-h`, 或 `--help` 时, 显示帮助文本.**

**默认显示精简的帮助文档。**
如果可以，尽量在运行 `myapp` 或 `myapp subcommand` 时显示帮助文档。
除非您的应用程序非常简单，并且默认情况下会执行明确的操作 (e.g. `ls`，或者您的程序是交互式读取输入的 (e.g. `cat`)。

精简的帮助文档应该只包含:

- 有关程序功能的说明。
- 一到两个调用示例。
- 启动参数的描述，除非有很多启动参数。
- 提示通过 `--help` 可以获取更多信息。

`jq` 是一个很好的典范。
当你输入 `jq` 时，会显示描述信息和使用示例，然后提示你可以通过 `jq --help` 获取完整的参数列表：

```
$ jq
jq - commandline JSON processor [version 1.6]

Usage:    jq [options] <jq filter> [file...]
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

**通过 `-h` 和 `--help` 显示完整的帮助信息.**
以下的输入都应该显示帮助信息:

```
$ myapp
$ myapp --help
$ myapp -h
```

如果一些命令和参数没有出现在默认帮助信息中，那么应该在该命令或参数的后面通过 `-h` 可以获取该命令或参数的帮助信息。

如果你的程序时类`git`程序，以下的情况也应提供帮助信息：

```
$ myapp help
$ myapp help subcommand
$ myapp subcommand --help
$ myapp subcommand -h
```

**提供反馈和问题的支持途径.**
一个网站或GitHub链接出现在顶级帮助文档中，这是很常见的做法。

**在帮助文本中提供Web版的文档链接**
如果您有相关子命令的特定页面或锚点，那就直接链接到该页面或锚点。
如果在Web上有更详细的说明文档，或者对一些命令参数进行进一步解释，则此功能特别有用。

**提供使用示例.**
用户倾向于使用示例而不是其他形式的文档，因此请首先在帮助页面中显示使用示例，尤其是常见的一些常见用法。如果可以帮助解释此示例的功能，也请显示实际输出结果。

您可以通过一系列示例为复杂的使用打下基础。
<!-- TK example? -->

**如果您有大量示例，请将它们放在其他地方,** 例如Web上.
拥有详尽的高级示例很有帮助，但是最好不要让帮助文本太长。

对于更复杂的用例，例如与其他工具集成时，您可能需要编写完整的教程。

**不要去管man page.**
如果您遵循这些准则获取帮助和文档，那么根本就不需要man page。
没有太多的人使用man page，并且man page在Windows上也无法使用。
如果您的CLI框架和程序包管理器使输出man page变得容易，那么可以加入man page，否则，您最好把时间用在改进Web文档和内置帮助文本上去。

_引用: [12 Factor CLI Apps](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46)._

**如果你的帮助文本长, pipe it through a pager.**
This is one useful thing that `man` does for you.
参见下面的“输出”一章

**在帮助文本的开头显示最常用的参数和命令.**
当有许多参数的时候，首先显示最常用的部分参数。
例如，Git命令首先显示用于入门的命令以及最常用的子命令：

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

**Use formatting in your help text.**
Bold headings make it much easier to scan.
But, try to do it in a terminal-independent way so that your users aren't staring down a wall of escape characters.

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

**If the user did something wrong and you can guess what they meant, suggest it.**
For example, `brew update jq` tells you that you should run `brew upgrade jq`.

You can ask if they want to run the suggested command, but don’t force it on them.
For example:

```
$ heroku pss
 ›   Warning: pss is not a heroku command.
Did you mean ps? [y/n]:
```

Rather than suggesting the corrected syntax, you might be tempted to just run it for them, as if they’d typed it right in the first place.
Sometimes this is the right thing to do, but not always.

Firstly, invalid input doesn’t necessarily imply a simple typo—it can often mean the user has made a logical mistake, or misused a shell variable.
Assuming what they meant can be dangerous, especially if the resulting action modifies state.

Secondly, be aware that if you change what the user typed, they won’t learn the correct syntax.
In effect, you’re ruling that the way they typed it is valid and correct, and you’re committing to supporting that indefinitely.
Be intentional in making that decision, and document both syntaxes.

_Further reading: [“Do What I Mean”](http://www.catb.org/~esr/jargon/html/D/DWIM.html)_

**If your command is expecting to have something piped to it and `stdin` is an interactive terminal, display help immediately and quit.**
This means it doesn’t just hang, like `cat`.
Alternatively, you could print a log message to `stderr`.

### 输出 {#output}

**Human-readable output is paramount.**
Humans come first, machines second.
The most simple and straightforward heuristic for whether a particular output stream (`stdout` or `stderr`) is being read by a human is _whether or not it’s a TTY_.
Whatever language you’re using, it will have a utility or library for doing this (e.g. [Python](https://stackoverflow.com/questions/858623/how-to-recognize-whether-a-script-is-running-on-a-tty), [Node](https://nodejs.org/api/process.html#process_a_note_on_process_i_o), [Go](https://github.com/mattn/go-isatty)).

_Further reading on [what a TTY is](https://unix.stackexchange.com/a/4132)._

**Have machine-readable output where it does not impact usability.**
Streams of text is the universal interface in UNIX.
Programs typically output lines of text, and programs typically expect lines of text as input,
therefore you can compose multiple programs together.
This is normally done to make it possible to write scripts,
but it can also help the usability for humans using programs.
For example, a user should be able to pipe output to `grep` and it should do what they expect.

> “Expect the output of every program to become the input to another, as yet unknown, program.”
— [Doug McIlroy](https://homepage.cs.uri.edu/~thenry/resources/unix_art/ch01s06.html)

**If human-readable output breaks machine-readable output, use `--plain` to display output in plain, tabular text format for integration with tools like `grep` or `awk`.**
In some cases, you might need to output information in a different way to make it human-readable.
<!-- (TK example with and without --plain) -->
For example, if you are displaying a line-based table, you might choose to split a cell into multiple lines, fitting in more information while keeping it within the width of the screen.
This breaks the expected behavior of there being one piece of data per line, so you should provide a `--plain` flag for scripts, which disables all such manipulation and outputs one record per line.

**Display output as formatted JSON if `--json` is passed.**
JSON allows for more structure than plain text, so it makes it much easier to output and handle complex data structures.
[`jq`](https://stedolan.github.io/jq/) is a common tool for working with JSON on the command-line, and there is now a [whole ecosystem of tools](https://ilya-sher.org/2018/04/10/list-of-json-tools-for-command-line/) that output and manipulate JSON.

It is also widely used on the web, so by using JSON as the input and output of programs, you can pipe directly to and from web services using `curl`.

**Display output on success, but keep it brief.**
Traditionally, when nothing is wrong, UNIX commands display no output to the user.
This makes sense when they’re being used in scripts, but can make commands appear to be hanging or broken when used by humans.
For example, `cp` will not print anything, even if it takes a long time.

It’s rare that printing nothing at all is the best default behavior, but it’s usually best to err on the side of less.

For instances where you do want no output (for example, when used in shell scripts), to avoid clumsy redirection of `stderr` to `/dev/null`, you can provide a `-q` option to suppress all non-essential output.

**If you change state, tell the user.**
When a command changes the state of a system, it’s especially valuable to explain what has just happened, so the user can model the state of the system in their head—particularly if the result doesn’t directly map to what the user requested.

For example, `git push` tells you exactly what it is doing, and what the new state of the remote branch is:

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

**Make it easy to see the current state of the system.**
If your program does a lot of complex state changes and it is not immediately visible in the filesystem, make sure you make this easy to view.

For example, `git status` tells you as much information as possible about the current state of your Git repository, and some hints at how to modify the state:

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

**Suggest commands the user should run.**
When several commands form a workflow, suggesting to the user commands they can run next helps them learn how to use your program and discover new functionality.
For example, in the `git status` output above, it suggests commands you can run to modify the state you are viewing.

**Actions crossing the boundary of the program’s internal world should usually be explicit.**
This includes things like:

- Reading or writing files that the user didn’t explicitly pass as arguments (unless those files are storing internal program state, such as a cache).
- Talking to a remote server, e.g. to download a file.

**Increase information density—with ASCII art!**
For example, `ls` shows permissions in a scannable way.
When you first see it, you can ignore most of the information.
Then, as you learn how it works, you pick out more patterns over time.

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

**Use color with intention.**
For example, you might want to highlight some text so the user notices it, or use red to indicate an error.
Don’t overuse it—if everything is a different color, then the color means nothing and only makes it harder to read.

**Disable color if your program is not in a terminal or the user requested it.**
These things should disable colors:

- `stdout` or `stderr` is not an interactive terminal (a TTY).
  It’s best to individually check—if you’re piping `stdout` to another program, it’s still useful to get colors on `stderr`.
- The `NO_COLOR` environment variable is set.
- The `TERM` environment variable has the value `dumb`.
- The user passes the option `--no-color`.
- You may also want to add a `MYAPP_NO_COLOR` environment variable in case users want to disable color specifically for your program.

_Further reading: [no-color.org](https://no-color.org/), [12 Factor CLI Apps](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46)_

**If `stdout` is not an interactive terminal, don’t display any animations.**
This will stop progress bars turning into Christmas trees in CI log output.

**Use symbols and emoji where it makes things clearer.**
Pictures can be better than words if you need to make several things distinct, catch the user’s attention, or just add a bit of character.
Be careful, though—it can be easy to overdo it and make your program look cluttered or feel like a toy.

For example, [yubikey-agent](https://github.com/FiloSottile/yubikey-agent) uses emoji to add structure to the output so it isn’t just a wall of text, and a ❌ to draw your attention to an important piece of information:

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

**By default, don’t output information that’s only understandable by the creators of the software.**
If a piece of output serves only to help you (the developer) understand what your software is doing, it almost certainly shouldn’t be displayed to normal users by default—only in verbose mode.

Invite usability feedback from outsiders and people who are new to your project.
They’ll help you see important issues that you are too close to the code to notice.

**Don’t treat `stderr` like a log file, at least not by default.**
Don’t print log level labels (`ERR`, `WARN`, etc.) or extraneous contextual information, unless in verbose mode.

**Use a pager (e.g. `less`) if you are outputting a lot of text.**
For example, `git diff` does this by default.
Using a pager can be error-prone, so be careful with your implementation such that you don’t make the experience worse for the user.
You shouldn’t use a pager if `stdin` or `stdout` is not an interactive terminal.

A good sensible set of options to use for `less` is `less -FIRX`.
This does not page if the content fills one screen, ignores case when you search, enables color and formatting, and leaves the contents on the screen when `less` quits.

There might be libraries in your language that are more robust than piping to `less`.
For example, [pypager](https://github.com/prompt-toolkit/pypager) in Python.

### 错误 {#errors}

One of the most common reasons to consult documentation is to fix errors.
If you can make errors into documentation, then this will save the user loads of time.

**Catch errors and [rewrite them for humans](https://www.nngroup.com/articles/error-message-guidelines/).**
If you’re expecting an error to happen, catch it and rewrite the error message to be useful.
Think of it like a conversation, where the user has done something wrong and the program is guiding them in the right direction.
Example: “Can’t write to file.txt. You might need to make it writable by running ‘chmod +w file.txt’.”

**Signal-to-noise ratio is crucial.**
The more irrelevant output you produce, the longer it’s going to take the user to figure out what they did wrong.
If your program produces multiple errors of the same type, consider grouping them under a single explanatory header instead of printing many similar-looking lines.

**Consider where the user will look first.**
Put the most important information at the end of the output.
The eye will be drawn to red text, so use it intentionally and sparingly.

**If there is an unexpected or unexplainable error, provide debug and traceback information, and instructions on how to submit a bug.**
That said, don’t forget about the signal-to-noise ratio: you don’t want to overwhelm the user with information they don’t understand.
Consider writing the debug log to a file instead of printing it to the terminal.

**Make it effortless to submit bug reports.**
One nice thing you can do is provide a URL and have it pre-populate as much information as possible.

### 参数与标志 {#arguments-and-flags}

A note on terminology:

- _Arguments_, or _args_, are positional parameters to a command.
  For example, the file paths you provide to `cp` are args.
  The order of args is often important: `cp foo bar` means something different from `cp bar foo`.
- _Flags_ are named parameters, denoted with either a hyphen and a single-letter name (`-r`) or a double hyphen and a multiple-letter name (`--recursive`).
  They may or may not also include a user-specified value (`--file foo.txt`, or `--file=foo.txt`).
  The order of flags, generally speaking, does not affect program semantics.

**Prefer flags to args.**
It’s a bit more typing, but it makes it much clearer what is going on.
It also makes it easier to make changes to how you accept input in the future.
Sometimes when using args, it’s impossible to add new input without breaking existing behavior or creating ambiguity.

_Citation: [12 Factor CLI Apps](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46)._

**Have full-length versions of all flags.**
For example, have both `-h` and `--help`.
Having the full version is useful in scripts where you want to be verbose and descriptive, and you don’t have to look up the meaning of flags everywhere.

_Citation: [GNU Coding Standards](https://www.gnu.org/prep/standards/html_node/Command_002dLine-Interfaces.html)._

**Only use one-letter flags for commonly used flags,** particularly at the top-level when using subcommands.
That way you don’t “pollute” your namespace of short flags, forcing you to use convoluted letters and cases for flags you add in the future.

**Multiple arguments are fine for simple actions against multiple files.**
For example, `rm file1.txt file2.txt file3.txt`.
This also makes it work with globbing: `rm *.txt`.

**If you’ve got two or more arguments for different things, you’re probably doing something wrong.**
The exception is a common, primary action, where the brevity is worth memorizing.
For example, `cp <source> <destination>`.

_Citation: [12 Factor CLI Apps](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46)._

**Use standard names for flags, if there is a standard.**
If another commonly used command uses a flag name, it’s best to follow that existing pattern.
That way, a user doesn’t have to remember two different options (and which command it applies to), and users can even guess an option without having to look at the help text.

Here's a list of commonly used options:

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

**Make the default the right thing for most users.**
Making things configurable is good, but most users are not going to find the right flag and remember to use it all the time (or alias it).
If it’s not the default, you’re making the experience worse for most of your users.

For example, `ls` has terse default output to optimize for scripts and other historical reasons, but if it were designed today, it would probably default to `ls -lhFGT`.

**Prompt for user input.**
If a user doesn’t pass an argument or flag, prompt for it.
(See also: [Interactivity](#interactivity))

**Never _require_ a prompt.**
Always provide a way of passing input with flags or arguments.
If `stdin` is not an interactive terminal, skip prompting and just require those flags/args.

**Confirm before doing anything dangerous.**
A common convention is to prompt for the user to type `y` or `yes` if running interactively, or requiring them to pass `-f` or `--force` otherwise.

“Dangerous” is a subjective term, and there are differing levels of danger:

- **Mild:** A small, local change such as deleting a file.
  You might want to prompt for confirmation, you might not.
  For example, if the user is explicitly running a command called something like “delete,” you probably don’t need to ask.
- **Moderate:** A bigger local change like deleting a directory, a remote change like deleting a resource of some kind, or a complex bulk modification that can’t be easily undone.
  You usually want to prompt for confirmation here.
  Consider giving the user a way to “dry run” the operation so they can see what’ll happen before they commit to it.
- **Severe:** Deleting something complex, like an entire remote application or server.
  You don’t just want to prompt for confirmation here—you want to make it hard to confirm by accident.
  Consider asking them to type something non-trivial such as the name of the thing they’re deleting.
  Let them alternatively pass a flag such as `--confirm="name-of-thing"`, so it’s still scriptable.

Consider whether there are non-obvious ways to accidentally destroy things.
For example, imagine a situation where changing a number in a configuration file from 10 to 1 means that 9 things will be implicitly deleted—this should be considered a severe risk, and should be difficult to do by accident.

**If input or output is a file, support `-` to read from `stdin` or write to `stdout`.**
This lets the output of another command be the input of your command and vice versa, without using a temporary file.
For example, `tar` can extract files from `stdin`:

```
$ curl https://example.com/something.tar.gz | tar xvf -
```

**If a flag can accept an optional value, allow a special word like “none.”**
For example, `ssh -F` takes an optional filename of an alternative `ssh_config` file, and `ssh -F none` runs SSH with no config file. Don’t just use a blank value—this can make it ambiguous whether arguments are flag values or arguments.

**If possible, make arguments, flags and subcommands order-independent.**
A lot of CLIs, especially those with subcommands, have unspoken rules on where you can put various arguments.
For example a command might have a `--foo` flag that only works if you put it before the subcommand:

```
mycmd --foo=1 subcmd
works

$ mycmd subcmd --foo=1
unknown flag: --foo
```

This can be very confusing for the user—especially given that one of the most common things users do when trying to get a command to work is to hit the up arrow to get the last invocation, stick another option on the end, and run it again.
If possible, try to make both forms equivalent, although you might run up against the limitations of your argument parser.

**Allow sensitive argument values to be passed in via files.**
Let’s say your command takes a secret via a `--password` argument.
A raw `--password` argument will leak the secret into `ps` output and potentially shell history.
It’s easy to misuse.
Consider allowing secrets only via files, e.g. with a `--password-file` argument.
A `--password-file` argument allows a secret to be passed in discreetly, in a wide variety of contexts.

(It’s possible to pass a file’s contents into an argument in Bash by using `--password $(< password.txt)`.
Unfortunately, not every context in which a command is run will have access to magical shell substitutions.
For example, `systemd` service definitions, `exec` system calls, and some `Dockerfile` command forms do not support the substitutions available in most shells.
What’s more, this approach has the same security issue of leaking the file’s contents into places like the output of `ps`.
It’s best avoided.)

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

能直接复制，但**有 3 处指代会自相矛盾**，未来你自己看会懵。建议微调后再贴。

## 为什么不能原样贴

这份 README 是站在 **lj 仓库**视角写的，里面有几句"本仓库（lj）…"。你贴到 1208 的"复刻保留仓库"里，就会变成"本仓库（lj）"——可你人在 1208 仓库，逻辑打架。

具体 3 处：

| 位置 | 原文 | 问题 |
|---|---|---|
| 标题 | （lj 原始验证版） | 在 1208 仓库里叫这名，容易搞混是哪个仓库 |
| 〇节第 1 条 | "本仓库（lj）是原始版本" | 本仓库是 1208 复刻，不是 lj |
| 一节"验证人" | lj| 应注明"原验证人在 lj，本仓库为复刻保留" |

其余全部照用，一个字不用改。

## 微调版（1208 复刻保留仓库专用，直接全选粘贴）

```
# Win11 ISO 自动构建器（1208 复刻保留版）

> 一句话核心：本仓库代码复刻自 lj 仓库，已在 Windows 11 Build 29695.1001 上完整构建成功（验证记录在 lj 仓库）。
> 本仓库的 main.yml 与原版**一字未改**，改名字方法和 lj 完全一样，见第四节。

## 〇、代码来源与两仓库关系（前提声明，必读）
- 本仓库（Jason1208751233 名下的复刻保留仓库）是**复刻版**：把 lj 仓库的 main.yml **强制原样复制**过来，一个字也没有改动，可逐行对照验证。
- 原始版仓库：lj804028CMGIST 的仓库（下称"lj 仓库"），是代码首次跑通的地方，已验证 Windows 11 Build 29695.1001 构建成功。
- 本仓库代码与 lj 仓库完全一致（genisoimage 补丁版、Release 标签写死 v29695），因此 lj 的验证结论对本仓库同样成立。
- Jason1208751233 名下还有另一个"改进版"仓库（mainyamlneedtochangewindowsnamefromuupdump），已升级为 xorriso + 自适应版本号，与本仓库并存、互不影响。
- 本文件第三节的使用教程两仓库通用；本版与改进版的详细区别见第五节。

---
## 一、已验证成功的运行记录（重要，勿删）
| 项目 | 内容 |
|---|---|
| 已验证的 Windows 版本 | Windows 11 Build 29695.1001（来自 uupdump.net） |
| 使用的 Release 标签 | v29695 |
| 构建产物 | win11.iso.part-00 至 part-0X 加 SHA256SUMS.txt（每块约 1.9GB） |
| 构建总耗时 | 约 40 到 55 分钟 |
| 首次成功日期 | （待填写：以 lj 仓库 Actions 页面绿勾那次为准） |
| 验证人 | lj804028CMGIST（原验证），本仓库为复刻保留，代码未改 |
> 这条记录的作用：证明"这份代码 + 这个 uup.zip + GitHub Actions 环境"的组合当时是通的。
> 以后跑挂了先对照这里，排查是不是 uupdump 改了脚本或 GitHub 环境变了，而不是怀疑代码本身。
> 本仓库如果也跑了一次，把那次日期补到第七节即可。

---
## 二、每一步在干什么（原理备忘）
| 步骤 | 作用 |
|---|---|
| Checkout | 把仓库（含 uup.zip）拉到构建机 |
| Free disk space | 删掉构建机预装的大软件，腾出约 14GB 放 ISO |
| Install dependencies | 安装 cabextract、wimtools、genisoimage、chntpw、aria2、unzip |
| Patch genisoimage | 手写一个包装脚本顶替 genisoimage，自动附加 allow-limited-size 参数，解除"单文件最大 4GB"限制。install.wim 通常 4 到 5GB，不打补丁必报 larger than 4GiB-1 错误 |
| Run UUP dump script | 解压 uup.zip，按清单从微软官方服务器下载散件，wimlib 合成，最后 genisoimage 打包成 ISO。最耗时的一步，约 25 到 35 分钟 |
| Locate and split ISO | 递归找最大的 ISO，改名 win11.iso，切成 1900MB 分块，生成 SHA256 校验文件 |
| Publish to Release | 先删旧标签 v29695 再重建同名 Release 并上传。注意这是覆盖式发布，每次构建都覆盖 v29695，旧版本不留历史 |

---
## 三、完整使用教程（两仓库通用）
### 3.1 获取 uup.zip
1. 打开 uupdump.net（https://uupdump.net）
2. 首页 Latest builds 或搜索版本号，选想要的 Windows 11 版本
3. 语言选 Chinese (Simplified)，一路 Next
4. 下载方式页选 Download the conversion script (Linux)，注意是 Linux 不是 Windows
5. 得到一个 zip，改名为 uup.zip（必须一字不差，全小写）
### 3.2 上传并运行
1. 仓库首页，Add file，Upload files，拖入 uup.zip，Commit changes
2. 特别注意：本代码上传 uup.zip 后**不会自动开跑**（触发器只监听 main.yml 的改动，这是原版设计）
3. 手动运行：Actions 标签，左侧选 build-iso，右侧 Run workflow，弹窗里再点一次绿色 Run workflow
4. 黄点表示进行中，绿勾表示完成，红叉表示失败
5. 等 40 到 55 分钟，完成后到 Releases 页面领成品
### 3.3 下载成品（按推荐排序）
方式 A：迅雷下载（实测最快，强烈推荐）
1. 在 Release 页面，右键某个 part 文件，复制链接地址
2. 打开迅雷，新建任务，粘贴链接，开始下载
3. 每个分块都这样复制粘贴下载一遍
实测经验：迅雷加挂梯子速度最快；只开迅雷不挂梯子也能下，稍慢；迅雷自动多线程，比浏览器快得多。
方式 B：浏览器直接下载。点文件名直接下，挂不挂梯子都能下，挂梯子更稳。
方式 C：镜像加速（失效就退回方式 A）。把复制的链接前面拼上镜像前缀：
```
原链接：
https://github.com/用户名/仓库名/releases/download/版本号/win11.iso.part-00
替换后（在整串链接最前面加上 https://gh-proxy.com/）：
https://gh-proxy.com/https://github.com/用户名/仓库名/releases/download/版本号/win11.iso.part-00
```
镜像站随时可能失效，失效就用方式 A 或 B。
### 3.4 拼回完整 ISO
所有 part 文件放同一个文件夹，然后：
Windows（在文件夹地址栏输入 cmd 回车，执行）：
```
copy /b win11.iso.part-00+win11.iso.part-01+win11.iso.part-02+win11.iso.part-03 win11.iso
```
有几个 part 就用加号接几个，顺序从小到大。
Linux 和 macOS：
```
cat win11.iso.part-* > win11.iso
```
### 3.5 校验（强烈建议）
先打开 SHA256SUMS.txt，第一列就是标准哈希值，然后核对：
```
Windows CMD：   certutil -hashfile win11.iso SHA256
PowerShell：    Get-FileHash win11.iso -Algorithm SHA256
Linux/macOS：   sha256sum -c SHA256SUMS.txt
```
哈希值一致 = 文件完好可以安装；不一致 = 下载坏了，重新下载坏的那一块。
### 3.6 安装
双击 win11.iso 挂载，运行里面的 setup.exe。想做 U 盘启动盘，用 Rufus（rufus.ie）或 Ventoy（ventoy.net）把 ISO 写入 8GB 以上的 U 盘。

---
## 四、★ 以后换新版本：必须手改的 4 个字符串（本版特有，重点）
本版代码的版本号是写死的（这正是它和改进版的根本区别）。
换新版本时，除了上传新 uup.zip，还必须改 main.yml 最后一步 Publish to Release 里的 4 处：
打开 .github/workflows/main.yml（铅笔图标编辑），找到 Publish to Release 步骤：
1. 删除行：`gh release delete v29695 --cleanup-tag --yes || true`
   把其中 v29695 改成新标签，例如 v30000
2. 创建行：`gh release create v29695`
   改成和第 1 处相同的新标签，两处必须一致
3. 标题：`--title "Win11 29695.1001"`
   改成新版本号
4. 说明：`--notes "UUP dump build 29695.1001"`
   改成新版本号说明
改完 Commit。然后必须手动 Run workflow（本代码传 uup.zip 不会自动触发）。
两种命名策略，选一种：
- 策略 A（保留历史，推荐）：每次用新标签，v29695 改成 v30000 以此类推，旧版本 Release 保留，能回溯
- 策略 B（只留最新）：4 处全不改，只换 uup.zip，代码会自动删旧建新原地覆盖。省事但历史丢失，且标题一直写着旧版本号，容易误导
不想每次改名字的话，用 Jason1208751233 名下的改进版仓库，版本号按构建时间自动生成，换版本只传 uup.zip 即可。
换版本完整动作清单（照着打勾）：
- [ ] 1. uupdump 下载新版本 Linux 脚本 zip，改名 uup.zip
- [ ] 2. 上传覆盖旧 uup.zip，Commit
- [ ] 3. （选策略 A 时）改 main.yml 里 4 个字符串，Commit
- [ ] 4. Actions，build-iso，Run workflow
- [ ] 5. 等绿勾，到 Releases 检查新版本号和产物
- [ ] 6. 在第七节构建历史表里补一行记录

---
## 五、本版（1208 复刻保留版）与改进版的区别
| 对比项 | 本版（复刻保留版，同 lj 原始版） | 改进版 |
|---|---|---|
| 大文件打包 | genisoimage 加包装脚本补丁，能用但写法绕 | 换 xorriso，一行 sed 替换，天然支持大文件 |
| 自动触发 | 只监听 main.yml，传 uup.zip 不会自动跑 | 监听 uup.zip，上传即自动构建 |
| 版本号 | 写死 v29695，换版本要手改 4 处 | 按构建日期时间加运行编号自动生成，永不撞车 |
| 换新版本操作 | 传 zip 加改 4 个字符串加手动跑 | 只传 zip，其他全自动 |
两份并存：本版是已验证的历史基准，改进版是日常使用推荐。

---
## 六、FAQ
问：跑出红叉了怎么办？
答：点进那次运行，点红叉步骤展开日志，报错原文复制发给 AI（ChatGPT 或 Claude）即可定位。最常见原因是 uupdump 官方源临时抽风，重跑一次就好。
问：为什么报 larger than 4GiB-1？
答：说明 Patch genisoimage 步骤没生效或被跳过。检查该步骤日志里 genisoimage --version 是否正常输出。
问：为什么上传 uup.zip 后没动静？
答：正常，本版不会自动触发。去 Actions 手动 Run workflow。
问：ISO 在哪？Code 页面怎么没有？
答：太大，不进仓库，只在 Releases 页面，且是分块形式。
问：Actions 收费吗？
答：本仓库是 Public，完全免费不限时长。
问：part 文件能直接用吗？
答：不能，是切块。按 3.4 拼回 win11.iso 才能挂载或刻录。

---
## 七、构建历史记录（每跑一次新版本就添一行）
| 日期 | Windows Build 号 | 使用的标签 | 结果 | 备注 |
|---|---|---|---|---|
| （待填写） | 29695.1001 | v29695 | 成功 | 首次验证在 lj 仓库（lj804028CMGIST） |
| | | | | |
> 添记录时顺手填上日期，未来的自己会感谢现在的你。
```

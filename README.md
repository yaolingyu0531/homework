# 平时作业 1 · 开源许可合规检测报告

- 课程：《开源创新思维》
- 姓名：童天瑜　学号：F25015439　班级：F250118
- 提交日期：2026-10-29

## 一、作业内容

选取 Apache 软件基金会旗下的 **Apache Commons Codec 1.18.0**，使用 **ScanCode Toolkit 32.5.0** 对其官方源码包做许可证、版权与包清单扫描，并逐项分析扫描结果，形成检测报告。

## 二、检测对象

| 项目 | 内容 |
| --- | --- |
| 项目名称 | Apache Commons Codec |
| 版本 | 1.18.0（2025-01-24 发布） |
| 源码包 | commons-codec-1.18.0-src.tar.gz（452 KB） |
| 规模 | 317 个文件 / 2.24 MB；Java 源文件 156 个（主代码 80 个 23 362 行，测试代码 76 个 20 746 行） |
| 声明许可 | Apache-2.0（LICENSE.txt 全文 + NOTICE.txt 版权声明） |

## 三、工具与执行

- 工具：ScanCode Toolkit 32.5.0（Apache-2.0，AboutCode 社区），许可证 + 版权 + 包清单 + 文件信息四类扫描
- 环境：Windows 11 x64、Python 3.12.14、独立虚拟环境
- 安装问题：pip 默认解析到 pluggy 1.7.0，与 plugincode 32.0.0 的类级 hookspec 注册不兼容，执行 `scancode --version` 报 `did not find any 'pre_scan' hooks`；降级 `pip install pluggy==1.6.0` 后恢复正常

```bash
scancode -clpi -n 4 --timeout 120 \
  --json-pp reports/codec-scan.json \
  --html    reports/codec-scan.html \
  --spdx-tv reports/codec-scan.spdx \
  --csv     reports/codec-scan.csv \
  commons-codec-1.18.0-src
```

扫描 364 个资源（317 个文件、47 个目录）、2.24 MB，耗时 45.6 s（6.95 文件/秒），错误数 0。

## 四、结论摘要

- **覆盖率**：309/317 个文件检出许可证（97.5%），其中 300 个文件为纯 `apache-2.0`
- **第三方条款（9 个文件）**：`public-domain`——MurmurHash2/3、Sha2Crypt、Blake3；`bsd-new`——PureJavaCrc32C（Intel，99 分）；`other-permissive`——DoubleMetaphoneTest（Kevin Atkinson）
- **误报与噪声**：changes.xml 中"Previous versions … Apache License 1.1"被记为 `apache-1.1`；README/CONTRIBUTING 的贡献者协议被记为 `generic-cla`
- **漏报**：Md5Crypt 注释中的"public domain（beer-ware）"未检出；Blake3 提到的 ISC 许可未检出
- **覆盖盲区**：8 个未检出文件中有 5 个二进制、1 个空文件、2 个纯文本；`bla.tar`/`bla.tar.xz` 未展开
- **依赖**：commons-lang3 3.17.0、commons-io 2.18.0、JUnit Jupiter（EPL-2.0），全部为 test 作用域，不进入发布构件
- **合规结论**：无 copyleft 传染、无许可证冲突，可作为 Apache-2.0 组件再分发；二进制分发时需附带 LICENSE、NOTICE 以及 Intel 与 Kevin Atkinson 的版权声明

## 五、文件清单

| 文件 | 说明 |
| --- | --- |
| [F25015439-童天瑜-作业1.docx](F25015439-童天瑜-作业1.docx) | 作业报告正文（4 页，按课程模板格式） |
| [reports/codec-scan.json](reports/codec-scan.json) | ScanCode 结构化结果（836 KB，含行号、命中规则、置信分） |
| [reports/codec-scan.html](reports/codec-scan.html) | 可视化报告（442 KB，浏览器直接打开） |
| [reports/codec-scan.spdx](reports/codec-scan.spdx) | SPDX 2.3 tag-value（102 KB，可导入 SBOM 平台） |
| [reports/codec-scan.csv](reports/codec-scan.csv) | 表格化清单（311 KB，便于 Excel 统计） |

> 报告与扫描结果均为本人在本地实际执行生成，未使用模拟数据。

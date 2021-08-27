---
title: jodconverter
---

## jodconverter

### 入门

​	概念：

​		jodconverter的全称是 java Document Converter ,它可以在不同的办公室格式之间转换文档。它利用了[Apache OpenOffice](https://www.openoffice.org/)或[LibreOffice](https://www.libreoffice.org/)， 

​	**JODConverter**自动执行OpenOffice / LibreOffice支持的所有转换。支持的转化包括： 

| 文件类型 | 输入格式                       | 输出格式                                      |
| -------- | ------------------------------ | --------------------------------------------- |
| 文本     | DOC，DOCX，ODT，OTT，RTF，TEXT | DOC，DOCX，HTML，ODT，OTT，PDF，PNG，RTF，TXT |
| 电子表格 | CSV，ODS，OTS，TSV，XLS，XLSX  | CSV，HTML，ODS，OTS，PDF，PNG，TSV，XLS，XLSX |
| 介绍     | ODP，OTP，PPT，PPTX            | HTML，ODP，OTP，PDF，PNG，PPT，PPTX，SWF      |
| 画画     | ODG，OTG                       | ODG，OTG，PDF，PNG，SWF                       |
| 其他     | HTML                           | DOC，DOCX，HTML，ODT，OTT，PDF，PNG，RTF，TXT |

### 系统要求

要使用**JODConverter，**您需要：

- Java运行时环境1.7或更高版本。
- [Apache OpenOffice](https://www.openoffice.org/)或[LibreOffice](https://www.libreoffice.org/) ; 通常建议使用最新的稳定版本。
- 任何可用于Java和Apache OpenOffice / LibreOffice的操作系统（Windows，Linux，Mac OS X，Solaris，FreeBSD等）。请参阅您计划用于操作系统要求的OOo分发的系统要求。
  - Apache OpenOffice [系统要求](https://www.openoffice.org/dev_docs/source/sys_reqs.html)。
  - LibreOffice [系统要求](https://www.libreoffice.org/get-help/system-requirements)。
- 要用作Web应用程序/ Web服务，您还需要一个servlet容器，例如[Apache Tomcat](https://tomcat.apache.org/)。

### 组件（配置）

#### officeHome

此属性设置将用于执行文档转换的办公室安装的office主目录。 

#### processManager

当JODConverter需要处理已启动的办公流程时，将使用流程管理器。当JODConverter启动办公室进程时，它必须检索已启动进程的PID，以便以后能够在需要时终止它。

 *默认值*：默认情况下，JODConverter将根据运行JODConverter的操作系统尝试查找最佳进程管理器。但是，如果在类路径中找到，则可以使用任何实现ProcessManager接口的进程管理器。

只有在启用了processManager后才可以使用jodconverter进行文件转换

其余属性请查看此链接<https://github.com/sbraconnier/jodconverter/wiki/Configuration> 

### 官方Java中使用dome

```java
File inputFile = new File("document.doc");
File outputFile = new File("document.pdf");

// Create an office manager using the default configuration.
// The default port is 2002. Note that when an office manager
// is installed, it will be the one used by default when
// a converter is created.
final LocalOfficeManager officeManager = LocalOfficeManager.install(); 
try {

    // Start an office process and connect to the started instance (on port 2002).
    officeManager.start();

    // Convert
    JodConverter
             .convert(inputFile)
             .to(outputFile)
             .execute();
} finally {
    // Stop the office process
    OfficeUtils.stopQuietly(officeManager);
}
```

### 本人整合springboot使用
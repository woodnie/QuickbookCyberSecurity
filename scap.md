# SCAP {#scap}

### 什么是SCAP {#scap-0}

[https://en.wikipedia.org/wiki/Security_Content_Automation_Protocol](https://en.wikipedia.org/wiki/Security_Content_Automation_Protocol)

[SCAP](https://baike.baidu.com/item/SCAP)（Security Content Automation Protocol：安全内容自动化协议）由NIST（National Institute of Standards and Technology：美国国家标准与技术研究院）提出，NIST期望利用SCAP解决三个棘手的问题：一是实现高层政策法规（如FISMA，ISO27000系列）等到底层实施的落地，二是将信息安全所涉及的各个要素标准化（如统一漏洞的命名及严重性度量），三是将复杂的系统配置核查工作自动化。SCAP是当前美国比较成熟的一套信息安全评估标准体系，其标准化、自动化的思想对信息安全行业产生了深远的影响。

NIST将SCAP 分为两个方面进行解释：Protocol（协议）与Content（内容）。Protocol是指SCAP由一系列现有的公开标准构成，这些公开标准被称为SCAP Element（SCAP元素）。Protocol规范了这些Element之间如何协同工作，Content指按照Protocol的约定，利用Element描述生成的应用于实际检查工作的数据。例如，FDCC（Federal Desktop CoreConfiguration：联邦桌面核心配置）、USGCB（United States Government ConfigurationBaseline：美国政府配置基线）等官方的检查单数据格式均为SCAP Content。

SCAP版本1.0包含以下六个SCAP元素：XCCDF、OVAL、CVE、CCE、CPE、CVSS。这些标准在SCAP产生之前都已经存在，并在各自的领域发挥着重要作用。其中一些标准我们可能之前有所了解，如CVE、CVSS。当SCAP将它们整合后，其整体标准化的优势变得十分明显。SCAP为安全工具实现标准化提供了解决方案：标准的输入数据格式、标准的处理方法和标准的输出数据格式，这非常有利于安全工具之间实现数据交换。<sup>[2]</sup> 

### **SCAP Element（SCAP元素）** {#scap-element-scap}

　　SCAP版本1.0包含以下六个SCAP元素：XCCDF、OVAL、CVE、CCE、CPE、CVSS。这些标准在SCAP产生之前都已经存在，并在各自的领域发挥着重要作用。其中一些标准我们可能之前有所了解，如CVE、CVSS。当SCAP将它们整合后，其整体标准化的优势变得十分明显。SCAP为安全工具实现标准化提供了解决方案：标准的输入数据格式、标准的处理方法和标准的输出数据格式，这非常有利于安全工具之间实现数据交换。

        SCAP Element可以分为以下三种类型：语言类，用来描述评估内容和评估方法的标准，包括了XCCDF和OVAL（1.2版SCAP添加了OCIL）；枚举类，描述对评估对象或配置项命名格式，并提供遵循这些命名的库，包括了CVE、CCE、CPE；度量类，提供了对评估结果进行量化评分的度量方法，对应的元素是CVSS（1.2版SCAP添加了CCSS）。

### **XCCDF与OVAL** {#xccdf-oval}

　　XCCDF 是由NSA（National Security Agency：美国国家安全局）与NIST共同开发，是一种用来定义安全检查单、安全基线、以及其他类似文档的一种描述语言。XCCDF使用标准的XML语言格式按照一定的格式（Schema）对其内容进行描述。在SCAP中，XCCDF完成两件工作：一是描述自动化的配置检查单（Checkilist），二是描述安全配置指南和安全扫描报告。一个XCCDF 文档包含一个或多个Profile，每个Profile可以理解为一个检查单。　　使用XCCDF无疑有许多好处，通过标准化能够让工具间的数据交换变得更加容易，能够很方便地根据目标系统的不同情况对检查项进行裁剪，而且无论是检查单还是检查结果能够很容易地转换成机器或人工能够读取的格式。　　OVAL由MITRE公司开发，是一种用来定义检查项、脆弱点等技术细节的一种描述语言。OVAL同样使用标准的XML格式来组织其内容。OVAL语言提供了足够的灵活性，可以用于分析Windows、Linux等各种操作系统的系统状态、漏洞、配置、补丁等情况，而且还能用于描述测试报告。OVAL使用简洁的XML格式清晰地对与安全相关的系统检查点作出描述，并且这种描述是机器可读的，能够直接应用到自动化的安全扫描中。OVAL的本质是Open（公开），这就意味着任何人都可以为OVAL的发展作出自己的贡献，共享知识和经验，避免重复劳动。　　XCCDF设计的目标是能够支持与多种基础配置检查技术交互。其中推荐的、默认的检查技术是MITRE公司的OVAL。在实际的SCAP应用中，XCCDF和OVAL往往是成对出现。

### **其他SCAP元素** {#scap-1}

　　CVE（Common Vulnerabilities and Exposures：通用漏洞及披露）是包含了公众已知的信息安全漏洞的信息和披露的集合。CCE（Common Configuration Enumeration：通用配置枚举）是用于描述计算机及设备配置的标准化语言。CPE（Common Platform Enumeration：通用平台枚举）是一种对应用程序、操作系统以及硬件设备进行描述和标识的标准化方案。　　CVSS（Common Vulnerability Scoring System：通用漏洞评分系统）是一个行业公开标准，其被设计用来评测漏洞的严重程度，并帮助确定其紧急度和重要度。在SCAP版本1.2中，引入了另外两个新标准：OCIL（Open Checklist Interactive Language：开放检查单交互语言）和CCSS（Common Configuration Scoring System：通用配置评分系统）。OCIL能够用来处理安全检查中需要人工交互反馈才能完成的检查项，CCSS作用与CVSS类似，不过CCSS关注的是系统配置缺陷的严重程度。

### **SCAP Content（SCAP内容）** {#scap-content-scap}

　　SCAP Content指的是遵照SCAP Protocol标准设计制作的用于自动化评估的数据，其实体是一个或多个XML文件。一般来说正式发布的SCAP Content至少包含两个XML文件，一个是XCCDF，另一个是OVAL，这些文件能够直接输入到各类安全工具中执行实际的系统扫描。Content中也可以包含描述其他SCAPElement的XML文件。按照SCAP Protocol标准组织的多个XML 文件也被称为SCAP Data Stream（SCAP数据流）。　　当前，无论是厂商、官方还是开源社区均提供了大量的SCAP Content。由于SCAP的开放性，这些资源为我们进行系统管理提供了很有价值的参考和极大的便利。　　厂商提供的SCAP Content　　Redhat在与SCAP整合方面做出了很多的努力，Redhat公司会定期发布针对其企业版本的Linux（RHEL）进行补丁及配置检查的OVAL文件，这些文件能够在http://www.redhat.com/security/data/oval下载。Fedora是基于Redhat Linux的一个免费的桌面发行版，它同样继承了Redhat在与SCAP整合方面的优势，大部分用于RHEL的SCAP内容能够很好地使用在Fedora系统上。Microsoft在其Security Compliance Manager（安全合规性管理器）中提供了大量用于检测Microsoft产品安全性的SCAP Content。　　NVD及其他官方提供的SCAP Content　　由NIST主导的NCP（National Checklist Program：美国国家检查单项目）现已积累了大量的用于系统安全性检查的SCAP Content，这些内容可以从NCP的官方网站[http://web.nvd.nist.gov/view/ncp/repository](http://web.nvd.nist.gov/view/ncp/repository)处得到。NVD（National Vulnerability Database：美国国家漏洞库）中所有的漏洞均使用SCAP标准中的OVAL、CVE与CCE描述，可以从其官方网站[http://nvd.nist.gov](http://nvd.nist.gov/)获取这些资源。此外，NIST还提供了FDCC、USGCB等项目的SCAP内容。

### NIST-SCAP {#nist-scap}

[https://csrc.nist.gov/projects/security-content-automation-protocol/](https://csrc.nist.gov/projects/security-content-automation-protocol/)

### open-scap {#open-scap}

[https://www.open-scap.org/](https://www.open-scap.org/)

### CN-SCAP {#cn-scap}

[http://www.scap.org.cn/](http://www.scap.org.cn/)
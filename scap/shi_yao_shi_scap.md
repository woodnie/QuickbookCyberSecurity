## 什么是SCAP {#scap}

[https://en.wikipedia.org/wiki/Security_Content_Automation_Protocol](https://en.wikipedia.org/wiki/Security_Content_Automation_Protocol)

[SCAP](https://baike.baidu.com/item/SCAP)（Security Content Automation Protocol：安全内容自动化协议）由NIST（National Institute of Standards and Technology：美国国家标准与技术研究院）提出，NIST期望利用SCAP解决三个棘手的问题：一是实现高层政策法规（如FISMA，ISO27000系列）等到底层实施的落地，二是将信息安全所涉及的各个要素标准化（如统一漏洞的命名及严重性度量），三是将复杂的系统配置核查工作自动化。SCAP是当前美国比较成熟的一套信息安全评估标准体系，其标准化、自动化的思想对信息安全行业产生了深远的影响。

NIST将SCAP 分为两个方面进行解释：Protocol（协议）与Content（内容）。Protocol是指SCAP由一系列现有的公开标准构成，这些公开标准被称为SCAP Element（SCAP元素）。Protocol规范了这些Element之间如何协同工作，Content指按照Protocol的约定，利用Element描述生成的应用于实际检查工作的数据。例如，FDCC（Federal Desktop CoreConfiguration：联邦桌面核心配置）、USGCB（United States Government ConfigurationBaseline：美国政府配置基线）等官方的检查单数据格式均为SCAP Content。

SCAP版本1.0包含以下六个SCAP元素：XCCDF、OVAL、CVE、CCE、CPE、CVSS。这些标准在SCAP产生之前都已经存在，并在各自的领域发挥着重要作用。其中一些标准我们可能之前有所了解，如CVE、CVSS。当SCAP将它们整合后，其整体标准化的优势变得十分明显。SCAP为安全工具实现标准化提供了解决方案：标准的输入数据格式、标准的处理方法和标准的输出数据格式，这非常有利于安全工具之间实现数据交换。<sup>[2]</sup>
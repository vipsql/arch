# 30. Capacity evaluation - Storage

Date: 2017-05-25

## Status

Accepted

## Context

1. 一些需要高 IOPS 的服务，磁盘使用的是普通云盘，如，数据库，备份服务，图片服务等。

## Decision

1. 明确存储的使用场景；
2. 关注吞吐量，IOPS和数据首次获取时间。

我们的存储都是基于 Aliyun 的，他有以下类别及特点：

* nas(文件存储)
	* 使用场景
		* 负载均衡共享存储和高可用
		* 企业办公文件共享(**不支持本地挂载**)
		* 数据备份
		* 服务器日志共享
	* 价格及吞吐能力
		* 2/G/M SSD性能型 60M/s
		* **0.65/G/M 容量型 30M/s**
* disk(块存储、云盘) 
	* 使用场景
		* 普通云盘
			* 不被经常访问或者低 I/O 负载的应用场景
		* 高效云盘
			* 中小型数据库
			* 大型开发测试
			* Web 服务器日志
		* SSD 云盘
			* I/O 密集型应用
			* 中大型关系型数据库
			* NoSQL 数据库
	* 价格
		* 普通云盘 0.3/G/M 
		* 高效云盘 0.35/G/M
		* SSD 云盘 1/G/M
	* 吞吐量
		* 普通云盘 30MBps
		* 高效云盘 **80MBps**
		* SSD 云盘 256MBps
	* IOPS
		* 普通云盘 数百
		* 高效云盘 3000
		* SSD 云盘 20000
	* 访问延迟
		* 普通云盘 5 - 10 ms
		* 高效云盘 1 - 3 ms
		* SSD 云盘 0.5 - 2 ms
* oss(对象存储)
	* 使用场景
		* 图片和音视频等应用的海量存储
		* 网页或者移动应用的静态和动态资源分离
		* 云端数据处理
	* 价格及吞吐能力
		* **0.148/G/M 标准型 吞吐量大，热点文件、需要频繁访问的业务场景** 大概 50M/s，类似高效云盘
		* **0.08/G/M 低频访问型 数据访问实时，读取频率较低的业务场景** Object 存储最低 30 天
		* 0.06/G/M 归档型 数据恢复有等待时间，数据有存储时长要求 Object 存储最低 30 天
* oas(归档存储)
	* 使用场景
		* 低成本备份
		* 数据归档
		* 取代磁带
	* 价格及吞吐能力
		* 0.07/G/M 用户提取数据时能够容忍0-4小时的时间延迟

## Consequences

1. 加粗的信息可以重点查看下，同类别里比较推荐；
2. 我们选择时还要考虑价格及存储时长要求。

Refs:

* [Server request and upgrade: capacity evaluation][1]
* 云盘参数和性能测试方法：[https://help.aliyun.com/document\_detail/25382.html][2]

[1]:	0019-server-request-and-upgrade-capacity-evaluation.md
[2]:	https://help.aliyun.com/document_detail/25382.html
# subfinder-x - 新一代子域名爆破工具
![Version](https://img.shields.io/badge/Version-2.0-green)

## 工具简介
subfinder-x是一款高性能的子域名爆破工具，使用 Go 语言开发，专为信息收集和渗透测试场景设计。该工具能够快速发现目标域名的子域名，并支持多级子域名扫描、HTTP服务识别和指纹识别等功能。


## 使用方法

### 基本用法

| 参数 | 长参数 | 说明 | 默认值 |
| ---- | ------ | ---- | ------ |
| `-u` | `--domain` | 指定目标域名 | - |
| `-f` | `--file` | 用于指定包含多个目标域名的文件 | - |
| `-s` | `-` | 指定扫描的深度 | - |
| `-d` | `--dict` | 指定字典文件 | test.txt |
| `-fd` | `--fuzz_data` | 设置FUZZ数据 | - |
| `-x` | - | 启用HTTP扫描和指纹识别 | false |
| `-c` | - | 是否开启随机子域名检查 | true |
| `-fp` | - | 指定指纹库路径 | finger.json |
| `-n` | --next | 指定二级字典文件 | mini_names.txt |

#### 基本扫描

```bash
# 使用默认配置扫描目标域名
subfinder-x.exe -u example.com
```
![image](1.png)

#### 使用自定义字典

```bash
# 使用自定义字典文件进行扫描
subfinder-x.exe -u example.com -d subdict.txt
```

#### 深度扫描

```bash
# 设置深度扫描的深度为2，默认为5
subfinder-x.exe -u example.com -s 2
```

#### 使用FUZZ模式

```bash
# 使用FUZZ模式进行定向位置扫描
subfinder-x.exe -u example.com -fd "test-FUZZ" -d subdict.txt
```
![image](2.png)

#### 启用HTTP扫描

```bash
# 启用HTTP服务扫描和指纹识别
subfinder-x.exe -u example.com -x
```
![image](3.png)

## 高级功能说明

### 深度扫描

每发现一个子域名，如果其深度小于设置的最大深度（默认为5），就会将其添加到列表中，然后使用二级字典对其进行扫描。

### FUZZ模式

FUZZ模式允许用户定义特定的子域名模式，工具会将字典中的每个词替换到 "FUZZ" 的位置。这对于定向扫描特定模式的子域名非常有用。

### HTTP服务扫描

启用HTTP服务扫描后，工具会对发现的子域名进行HTTP和HTTPS请求，识别Web服务，并提取标题和指纹信息。

### 随机子域名检查

通过生成随机子域名并查询其解析结果，来判断目标域名是否启用了泛解析，用于识别和过滤掉泛解析域名，提高扫描结果的准确性。如果随机子域名能够解析到IP，则将这些IP添加到黑名单中。

如果目标域名确实没有泛解析，可以考虑禁用此功能以加快扫描速度

-n 参数用于指定二级字典文件（默认mini_names.txt），主要用于深度扫描时使用。例如，如果发现了 www.example.com ，工具会使用二级字典中的每个条目尝试扫描 *.www.example.com 。

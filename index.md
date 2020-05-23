## ZYSama

You can use the [editor on GitHub](https://github.com/68823613/ZYSama.github.io/edit/master/index.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/68823613/ZYSama.github.io/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.

# TLS1.3和TLS1.2的区别

1、密钥协商机制改变，TLS1.3借助扩展进行密钥交换，TLS1.3只需要三次握手交互，TLS1.2则需要四次握手。

首先看TLS1.2，它通过`KeyExchange`进行密钥协商，即`ServerKeyExchange`和`ClientKeyExchange`，那么密钥交换本身就需要一个交互来回，所以总共有四次握手交互。

![TLS1.2](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy84MDA5Ny1kZWI4M2JhMzM3ZDFkYTgzLnBuZw?x-oss-process=image/format,png)

再看TLS1.3，通过`ClientHello`和`ServerHello`的扩展进行密钥交换，那么就省去了1.2版本中`KeyExchange`的过程，也就省去了一次握手。

![TLS1.3](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy84MDA5Ny1jYTUzZDNkMDg2NDUwYjZiLnBuZw?x-oss-process=image/format,png)

2、添加**0-RTT模式**，**以某些安全属性为代价**。

当`client`和`server`共享一个预共享密钥`PSK`（从外部获得或通过一个以前的握手获得）时，TLS 1.3允许`client`在第一个发送出去的消息的`early data`中携带数据，`client`使用这个`PSK`来认证`server`并加密`early data`。即在握手之前就有了`PSK`时，在第一次发送`ClientHello`时就可以发送加密数据，达到0-RTT数据传输的目的。

![0-RTT](https://img-blog.csdnimg.cn/20200504154631214.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzM2MDU4OTI3,size_16,color_FFFFFF,t_70)



但是，`0-RTT`数据的**安全性会降低**（缺少前向安全）。

3、`ServerHello`之后的所有握手消息都被加密，引入了加密扩展`EncryptedExtension`。

4、全面使用`ECC`密码算法，删除不具有前向安全的密码套件。

5、其他还包括新的密钥派生函数，删除多余的报文消息（`ChangeCipherSpec`，但我抓包测试时还是有），以及其他算法的改进。

[更详细的内容可以参考RFC文档](https://datatracker.ietf.org/doc/rfc8446/)



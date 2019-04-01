# Trickuri

Manual tests for URL Spoofing scenarios
手动测试URL欺骗场景

This tool is designed to allow testing of applications' display of URLs.
此工具的设计目的是允许测试应用程序的url显示。

## Background
## 背景

URLs are often the only source of identity information available when making
security decisions in a web browser or other context, but URL syntax is
complicated and subject to a wide variety of spoofing attacks. The Chromium
project maintains a [set of URL display guidelines][url-display-guidelines] that
covers best practices and pitfalls of URL display. Trickuri allows easy exercise
of common sources of spoofing vulnerabilities to ensure applications are robust
in their display of URLs.
URLs通常是web浏览器或其他安全决策下的惟一可用的身份信息来源，但URL语法是复杂且容易受到各种欺骗攻击。
The Chromium计划维护一个[set of URL display guidelines][url-display-guidelines] 用来
介绍URL显示的最佳实践和陷阱。Trickuri允许简单的操作，以确保应用程序是健壮的url显示。

[url-display-guidelines]: https://chromium.googlesource.com/chromium/src/+/master/docs/security/url_display_guidelines/url_display_guidelines.md

## Implementation
## 实现

Trickuri is configured as a proxy server for the client application under test.
All of the client's HTTP requests are sent to the proxy. The proxy returns HTML
content such that the behavior of the client application can be tested. For
instance, for Chrome itself, the tester can examine the content of the omnibox
to verify that the origin is visible and unambiguously identified to the user.
被配置为测试中的客户机应用程序的代理服务器。客户端的所有HTTP请求都发送到代理。代理返回HTML
内容，以便可以测试客户机应用程序的行为。例如，对于Chrome本身，测试人员可以检查omnibox的内容
以验证源文件是可见的，并且用户可以清楚地被识别。

## Testcases
## 测试点

Files in the ```testcases``` folder will be served as if they were served from any
URL, i.e. with the proxy running, visiting example.com/samplepathtest will serve
testcases/samplepathtest. Additional test cases can be added to the ```testcases```
folder.

## Running
## 运行

To run Trickuri, run ```go run trickuri.go``` in the source directory.
启动Trickuri, 在源目录执行 ```go run trickuri.go``` 即可。

## Proxy configuration
## 代理配置

The proxy may be configured in one of two ways:
代理配置可以采用下列2种方法之一|：

1.  As a "static proxy", running on port ```1270``` (by default) of the machine
    running the proxy.
2.  As an "autoconfigured proxy" where the client pulls
    ```http://<IP/hostname of computer running Trickuri>:1270/proxy.pac``` as the
    proxy determination script.

The advantage of the latter configuration is that it allows the proxy to specify
that it should be bypassed for certain URLs, e.g. those used by SafeBrowsing,
component updates, etc. Such bypasses help limit the impact of the proxy on the
system under test.
后一种配置的优点是允许代理指定它应该绕过某些url，例如安全浏览使用的url，
组件更新,等等。此类旁路有助于限制代理被测试系统影响。

See https://www.chromium.org/developers/design-documents/network-settings for
instructions on configuring proxy settings if you are testing Chrome with
Trickuri.
查看https://www.chromium.org/developers/design-documents/network-settings
有关如何配置代理设置的说明，如果您正在使用Trickuri测试Chrome。

## Certificate configuration
## 配置证书
In order for the tool to be able to intercept HTTPS requests, its root
certificate needs to be trusted by the application being tested. If you are
testing Chrome, this means that you should import the Trickuri root certificate
into your OS trust store. The root certificate can be downloaded from
http://localhost:1270/root.cer once Trickuri is running.
为了让工具能够拦截HTTPS请求，正在测试的应用程序需要信任它的根证书。如果你在
测试Chrome，这意味着您应该导入Trickuri根证书进入OS信任存储区。一旦 Trickuri 运行。
可以从http://localhost:1270/root.cer 下载根证书。

## Flags
## 标记

```-p``` Sets the port in which the tool will listen, defaults to ```1270```.
```-p``` 设置工具将侦听的端口，默认为 ```1270```.

```-h``` Sets the port for the https server, defaults to ```8443```.
```-h``` 设置http服务器端口，默认为 ```8443```.

```-d``` Sets the directory for certificate storage, defaults to ```~/.config/trickuri```
```-d``` 设置证书存储目录，默认为```~/.config/trickuri```

## Introduction

#### Install and configure the latest version of funcraft

The following examples use the Fun tool for debugging and deploying functions. First install and configure the latest version of funcraft

```bash
npm install @alicloud/fun -g
```

> You need to install nodejs 8.6.0 or above

Before using, we need to configure first, by typing fun config, and then configure the Account ID, Access Key ID, Secret Access Key, and Default Region Name in turn according to the prompt:

![](https://img.alicdn.com/tfs/TB1qp7Oy7Y2gK0jSZFgXXc5OFXa-622-140.png)

#### Cloning sample project

```bash
git clone https://github.com/awesome-fc/fc-custom-demo
```

> If you don't have git installed, you can type [https://github.com/awesome-fc/fc-custom-demo](https://github.com/awesome-fc/fc-custom-demo) into your browser and then click the download button to download directly.

#### Index

- [Python37](#python37)
- [Python37-HTTP](#python37-http)
- [SpringBoot](#springboot)

<a name="python37"></a>

## Python37

Support single instance multiple concurrency

#### Deploy Function

```bash
sam@iZj6c895xh98:~/fc-custom-demo cd python37-demo
sam@iZj6c895xh98:~/fc-custom-demo/python37-demo  fun install -v
using template: template.yml
start installing function dependencies without docker

building python37-demo/fc-python37
Funfile exist, Fun will use container to build forcely
Step 1/3 : FROM registry.cn-beijing.aliyuncs.com/aliyunfc/runtime-custom:build-1.9.4
 ---> 91c38ebf44f1
Step 2/3 : RUN fun-install pip install flask
 ---> Using cache
 ---> bc3e7d2a77c8
Step 3/3 : RUN fun-install pip install gunicorn
 ---> Using cache
 ---> 119fe14306b6
sha256:119fe14306b6548f9c4a8642cb214f7352ecb264e806ca8e000a8a6e1f0612ec
Successfully built 119fe14306b6
Successfully tagged fun-cache-9cf56df5-03b2-4119-9dab-69178cf590ff:latest
copying function artifact to /Users/sam/tmp/fc-custom-demo/python37-demo

Install Success

sam@iZj6c895xh98:~/fc-custom-demo/python37-demo fun deploy -y
···
Waiting for service python37-demo to be deployed...
        Waiting for function fc-python37 to be deployed...
                Waiting for packaging function fc-python37 code...
                The function fc-python37 has been packaged. A total of 369 files were compressed and the final size was 1.5 MB
        function fc-python37 deploy success
service python37-demo deploy success
```

#### Invoke Function

```bash
sam@iZj6c895xh98:~/fc-custom-demo/python37-demo   fun invoke -e "Hello World"
...
========= FC invoke Logs begin =========
FC Invoke Start RequestId: 3b34c887-e0d1-4015-afbd-63a387f6044c
b'Hello World'
FC Invoke End RequestId: 3b34c887-e0d1-4015-afbd-63a387f6044c

Duration: 3.17 ms, Billed Duration: 100 ms, Memory Size: 512 MB, Max Memory Used: 54.58 MB
========= FC invoke Logs end =========

FC Invoke Result:
Hello World
```

<a name="python37-http"></a>

## Python37-HTTP

Support single instance multiple concurrency

#### Deploy Function

```bash
sam@iZj6c895xh98:~/fc-custom-demo cd python37-http-demo
sam@iZj6c895xh98:~/fc-custom-demo/python37-http-demo  fun install -v
using template: template.yml
start installing function dependencies without docker

building python37-demo/fc-python37-http
Funfile exist, Fun will use container to build forcely
Step 1/3 : FROM registry.cn-beijing.aliyuncs.com/aliyunfc/runtime-custom:build-1.9.9
 ---> 2479a0c4aab6
Step 2/3 : RUN fun-install pip -i https://pypi.tuna.tsinghua.edu.cn/simple install flask
 ---> Using cache
 ---> 6183f1188726
Step 3/3 : RUN fun-install pip -i https://pypi.tuna.tsinghua.edu.cn/simple install gunicorn
 ---> Using cache
 ---> 181ddb1702f0
sha256:181ddb1702f01b303676d8736d2c09f4cafe02c560fc54c6f6b780ee43d6346b
Successfully built 181ddb1702f0
Successfully tagged fun-cache-44a38469-67eb-4c2e-9784-66080b5cf8c5:latest
copying function artifact to /Users/sam/gitpro/fc-custom-demo/python37-http-demo

Install Success

sam@iZj6c895xh98:~/fc-custom-demo/python37-http-demo  fun deploy -y
···
Waiting for service python37-demo to be deployed...
        Waiting for function fc-python37-http to be deployed...
                Waiting for packaging function fc-python37-http code...
                The function fc-python37-http has been packaged. A total of 368 files were compressed and the final size was 1.49 MB
                Waiting for HTTP trigger http_t to be deployed...
                triggerName: http_t
                methods: [ 'GET', 'POST' ]
                trigger http_t deploy success
        function fc-python37-http deploy success
service python37-demo deploy success

Detect 'DomainName:Auto' of custom domain 'my_domain'
Fun will reuse the temporary domain http://37584684-123456789.test.functioncompute.com, expired at 2020-12-20 15:18:04, limited by 1000 per day.

Waiting for custom domain my_domain to be deployed...
custom domain my_domain deploy success
```

#### Invoke Function

```bash
sam@iZj6c895xh98:~/fc-custom-demo/python37-demo  curl -v http://37584684-123456789.test.functioncompute.com
...
* Connected to 37584684-1186202104331798.test.functioncompute.com (47.111.48.14) port 80 (#0)
> GET / HTTP/1.1
> Host: 37584684-1186202104331798.test.functioncompute.com
> User-Agent: curl/7.64.1
> Accept: */*
>
< HTTP/1.1 200 OK
< Access-Control-Expose-Headers: Date,x-fc-request-id,x-fc-error-type,x-fc-code-checksum,x-fc-invocation-duration,x-fc-max-memory-usage,x-fc-log-result,x-fc-invocation-code-version
< Content-Length: 12
< Content-Type: text/html; charset=utf-8
< X-Fc-Code-Checksum: 6465955828983873790
< X-Fc-Invocation-Duration: 3
< X-Fc-Invocation-Service-Version: LATEST
< X-Fc-Max-Memory-Usage: 85.20
< X-Fc-Request-Id: 1a26ecd3-3a6b-4b1c-9c33-e2eb0d71c7ec
< Date: Thu, 10 Dec 2020 07:23:21 GMT
<
* Connection #0 to host 37584684-1186202104331798.test.functioncompute.com left intact
Hello World!* Closing connection 0
```


<a name="springboot"></a>

## Springboot

#### Deploy Function

```bash
sam@iZj6c895xh98:~/fc-custom-demo cd spring-boot-demo
sam@iZj6c895xh98:~/fc-custom-demo/spring-boot-demo  ./mvnw package
[INFO] Scanning for projects...
[INFO]
[INFO] --------------------------< com.example:demo >--------------------------
[INFO] Building demo 0.0.1-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
...
[INFO]  T E S T S
...
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  25.332 s
[INFO] Finished at: 2021-03-25T14:56:29+08:00
[INFO] ------------------------------------------------------------------------
sam@iZj6c895xh98:~/fc-custom-demo/spring-boot-demo  fun deploy -y
...
Waiting for service springboot to be deployed...
        Waiting for function helloworld to be deployed...
                Waiting for packaging function helloworld code...
                The function helloworld has been packaged. A total of 3 files were compressed and the final size was 14.33 MB
                Waiting for HTTP trigger httpTrigger to be deployed...
                triggerName: httpTrigger
                methods: [ 'GET', 'POST', 'PUT' ]
                trigger httpTrigger deploy success
        function helloworld deploy success
service springboot deploy success

Detect 'DomainName:Auto' of custom domain 'my_domain'
Request a new temporary domain ...
The assigned temporary domain is http://46656184-1986114430573743.test.functioncompute.com，expired at 2021-04-04 15:09:44, limited by 1000 per day.
Waiting for custom domain my_domain to be deployed...
custom domain my_domain deploy success
```

#### Invoke Function

```bash
sam@iZj6c895xh98:~/fc-custom-demo/spring-boot-demo  curl http://46656184-1986114430573743.test.functioncompute.com
Hello, World!
```

#### Deep Dive

**Q: What's the genral procedure?**

A: To implement a custom runtime, the key component is a file named `bootstrap`, a binary or an executable script. It should start an HTTP server that listens on port `9000` and serve the HTTP requests on the following paths.

- `/invoke`: The handler for event trigger.
- `/http-invoke`: The handler for HTTP trigger.
- `/initialize`: The handler of initialization.

Behind the curtain, the FC (Function Compute) system will execute the `bootstrap`, connect to it through port 9000, and relay the request to it via HTTP.

**Q: What is `Dockerfile.build` for?**

A: It is noteworthy that the `bootstrap` will be executed in [a specific environment](https://github.com/aliyun/fc-docker/blob/master/custom/build/Dockerfile) ([more details](https://help.aliyun.com/document_detail/132044.html#h2-u6267u884Cu73AFu58833) in the doc), so we have to build the binary in the same environment. Hence we define the `Dockerfile.build` based on it and later compile the Rust code inside.

**Q: What will be uploaded?**

A: `make build` will create a directory named `pkg` and place the newly-created `bootstrap` into it. Then the `fun` CLI will automatically package `pkg` and deploy it onto FC.

**Q: Usage outside China?**

A: Currently, `Dockerfile.build` heavily relies on Chinese mirror for speeding up the installation. Specifically, Line 9-16 are the mirrors for Debian APT sources and Line 24-25 for installing Rust. Similarly, the `bootstrap/.cargo/config.toml` has also set a mirror source. Replace them with your local mirrors if you are residing outside China.

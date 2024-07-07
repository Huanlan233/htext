+++
title = "Minecraft Fabric高版本开发中指令的解析"
date = 2023-08-05T15:59:52+08:00
draft = false
categories = ["编程", "Minecraft"]
tags = ["Minecraft", "Fabric", "Java"]
slug = "7d161ded"
+++

从 1.19 之后 `getCommandManager().execute()` 中 `execute()` 第一个形参从 `ServerCommandSource` 类变成了 `ParseResult<ServerCommandSource>` 类了。

看了下，`ParseResult` 似乎是对指令解析的返回结果，包含 `CommandContext<ServerCommandSource>` 和 `ImmutableStringReader` 其他类或接口在内构成了一个解析的结果，导向消息的发送人、指令的内容和抛出的异常。

没有办法直接从 `CommandContext<ServerCommandSource>` 类中获取到解析结果，必须使用 `dispatcher` 指令调度器进行解析。

```java
ParseResults<ServerCommandSource> parseResults = dispatcher.parse(COMMAND, context.getSource()); // 对指令进行解析
dispatcher.execute(parseResults); // 执行指令
```
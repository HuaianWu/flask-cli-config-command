为 Flask CLI 增加 flask config 命令，用于显示应用的有效配置。

当前 Flask CLI 提供 run、shell、routes 等命令，但没有查看配置的命令。请新增 flask config 命令：

预期行为：
运行 flask config 显示应用合并后的有效配置（默认配置 + 用户通过 app.config 设置的值），每个键值一行。
支持按 key 过滤：flask config KEY 只显示该键的值；键不存在时报错并退出非 0。
支持 json 输出整个配置为 JSON。
输出按 key 排序；值用可读格式展示，与 app.config 中存储的一致。
不传参数且不加 json 时输出人类可读的 KEY = VALUE 列表。
命令在应用上下文内运行（能访问 current_app.config）

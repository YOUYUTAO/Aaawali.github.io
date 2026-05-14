# 浏览器烧录所需的二进制文件

本目录下的文件名必须与 `../manifest.json` 中的 `path` 一致。

从本机 ESP-IDF 工程 **`build/`** 目录复制（构建完成后）：

| 本目录文件名 | 来源（build 内路径） |
|-------------|---------------------|
| `bootloader.bin` | `bootloader/bootloader.bin` |
| `partition-table.bin` | `partition_table/partition-table.bin` |
| `ota_data_initial.bin` | `ota_data_initial.bin` |
| `xiaozhi.bin` | `xiaozhi.bin` |
| `generated_assets.bin` | `generated_assets.bin` |

复制命令示例（在工程根目录执行，PowerShell）：

```powershell
$dst = "docs/web-flash/firmware"
Copy-Item build/bootloader/bootloader.bin        "$dst/bootloader.bin"
Copy-Item build/partition_table/partition-table.bin "$dst/partition-table.bin"
Copy-Item build/ota_data_initial.bin             "$dst/ota_data_initial.bin"
Copy-Item build/xiaozhi.bin                      "$dst/xiaozhi.bin"
Copy-Item build/generated_assets.bin             "$dst/generated_assets.bin"
```

## 发布方式二选一

1. **随仓库 + GitHub Pages**  
   将上述 5 个文件提交到 `docs/web-flash/firmware/`，在仓库 Settings → Pages 中启用 **Deploy from branch**，文件夹选 **`/docs`**。  
   烧录页地址一般为：  
   `https://<你的用户名>.github.io/<仓库名>/web-flash/`

2. **二进制放在 GitHub Release**（减小仓库体积）  
   上传 5 个文件到 Release，把 `manifest.json` 里各 `path` 改成该文件的 **HTTPS 直链**。  
   注意：若直链域名与 Pages 不同，需目标服务器允许 **CORS**（GitHub `raw.githubusercontent.com` 对 release 资源不一定适合大文件直链；优先用 `releases/download/...` 并自行验证浏览器能否下载）。

## 体积说明

`xiaozhi.bin` 与 `generated_assets.bin` 较大。若不想提交进 Git，请用 Release + 修改 manifest 中的 URL。

根目录 `.gitignore` 已用 `!docs/web-flash/firmware/*.bin` 放行本目录下的固件，可直接 `git add docs/web-flash/firmware/*.bin`。

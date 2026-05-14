# 通过 GitHub Pages 提供「浏览器烧录」

本目录包含静态页 `index.html` 与 `manifest.json`，使用 [ESP Web Tools](https://esphome.github.io/esp-web-tools/)（`esp-web-install-button` + Web Serial）。

## 1. 准备固件文件

在本地 `idf.py build` 后，将 5 个 bin 复制到 `firmware/`（见 `firmware/README.md`）。

## 2. 启用 GitHub Pages

1. 打开 GitHub 仓库 **Settings → Pages**  
2. **Build and deployment**：Source 选 **Deploy from a branch**  
3. Branch 选你的默认分支（如 `main`），文件夹选 **`/docs`**  
4. 保存后等待几分钟，站点根地址为：  
   `https://<用户名>.github.io/<仓库名>/`

## 3. 烧录页地址

本页位于 `docs/web-flash/`，因此完整 URL 一般为：

`https://<用户名>.github.io/<仓库名>/web-flash/`

把该链接发给需要烧录的人即可。

## 4. 不想把大 bin 提交进 Git？

- 使用 **GitHub Releases** 上传 5 个文件，将 `manifest.json` 中各 `path` 改为 Release 直链（可参考 `manifest.release.example.json`）。  
- 若浏览器跨域下载失败，可把 manifest 与 bin 放在**同一域名**下（例如同一 Pages 站点子路径，或你自己的 CDN）。

## 5. 浏览器要求

- **Chrome** 或 **Edge** 桌面版（需支持 [Web Serial API](https://developer.mozilla.org/docs/Web/API/Web_Serial_API)）  
- 需 **HTTPS**（`github.io` 已满足）  
- 用户需允许网站访问 USB 串口

## 6. 与命令行烧录的对应关系

与 `build/flash_args` 一致：

| 偏移 | 文件 |
|------|------|
| 0x0 | bootloader |
| 0x8000 | partition table |
| 0xd000 | ota_data_initial |
| 0x20000 | xiaozhi.bin |
| 0x800000 | generated_assets.bin |

Flash 参数：`dio` / `80m` / `16MB`（由 ESP Web Tools 按芯片写入流程处理；若遇兼容问题可参考 ESP Web Tools 文档中的 `merge_bin` 方案）。

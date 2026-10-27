## 2. `android_device_oneplus_macan` 的 README.md

# TWRP device tree for OnePlus Ace 6T (macan)

OnePlus Ace 6T / OnePlus 15R（代号 macan）的 TWRP 设备树，配套 [yarp-macan](https://github.com/mzsy123/yarp-macan) 脚手架，用于构建 TWRP-Test `twrp-16.0`（YARP GUI2）recovery。

> 本项目 fork 自 [koaaN/android_device_oneplus_macan-orangefox](https://github.com/koaaN/android_device_oneplus_macan-orangefox)，由 OrangeFox 设备树改造为纯 TWRP 版。

## 设备规格

| 项目 | 值 |
| --- | --- |
| 代号 | macan |
| 国行型号 | OnePlus Ace 6T（PLR110） |
| 国际型号 | OnePlus 15R（CPH2767 / CPH2769 / CPH2771） |
| SoC | Snapdragon 8 Gen 5（SM8845） |
| 平台 | canoe |
| OTA 设备名 | OP6117L1 |
| 出厂系统 | Android 16 |

## 状态

- [x] 通过 GitHub Actions 编译，产出 `recovery.img`
- [ ] 真机显示 / 触摸
- [ ] FBE 解密
- [ ] 刷入 / 备份 / MTP（待真机验证）

> 本树是 twrp-16.0 + macan 的首次移植，上述真机功能尚未验证，属于实验性质。

## 构建

本树不单独构建，配合 [yarp-macan](https://github.com/mzsy123/yarp-macan) 脚手架使用：

1. Fork 本仓库，保持分支 `main`。
2. 在 `yarp-macan` 的 `sources.json` 中，`device` 段指向本仓库（`name` = `android_device_oneplus_macan`，`revision` = `main`）。
3. 运行 `yarp-macan` 的 Actions 即可。

内核在构建时自动从 [OnePlus-SM8850-Development/android_device_oneplus_macanc-kernel](https://github.com/OnePlus-SM8850-Development/android_device_oneplus_macanc-kernel)（`lineage-24.0` 分支）拉取，无需手动放置。

## 目录结构

.
├── AndroidProducts.mk # PRODUCT_MAKEFILES → twrp_macan
├── BoardConfig.mk # 核心硬件/分区/模块配置
├── device.mk # 设备级配置
├── twrp_macan.mk # PRODUCT_NAME := twrp_macan
├── recovery/ # recovery root（vendor 库、qseecomd、init rc 等）
├── security/ # OTA 证书
├── system.prop
└── vendorsetup.sh
代码

## 关键配置说明

- 内核：`TARGET_PREBUILT_KERNEL := device/oneplus/macan-kernel/images/kernel`（构建时自动 clone）。
- 解密：`TW_INCLUDE_CRYPTO_FBE` / `TW_INCLUDE_FBE_METADATA_DECRYPT` / `TW_USE_FSCRYPT_POLICY := 2`。
- 触摸 / 显示模块：`TW_LOAD_VENDOR_MODULES`（一加 `oplus_bsp_tp_*` 等）。
- 分区大小 `BOARD_SUPER_PARTITION_SIZE`、`BOARD_RECOVERYIMAGE_PARTITION_SIZE` 刷机前应按真机 `blockdev --getsize64` 实测修正。

## 致谢

- [koaaN/android_device_oneplus_macan-orangefox](https://github.com/koaaN/android_device_oneplus_macan-orangefox)
- [OnePlus-SM8850-Development](https://github.com/OnePlus-SM8850-Development)
- [TWRP-Test](https://github.com/TWRP-Test)

## 免责声明

本设备树产物未经真机验证，刷机可能变砖或丢失数据，请自行备份并自担风险。
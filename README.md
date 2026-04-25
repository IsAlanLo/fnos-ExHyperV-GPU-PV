## FNOS 1.1.23 ExHyper-V GPU-PV部署腳本

### 說明

本腳本適用於 **fnOS(1.1.23)** 搭配 **Linux 6.12.18 核心**的 Hyper-V 虛擬機環境，
根據[Ubuntu-22.04-Official.sh](https://github.com/Justsenger/ExHyperV/blob/main/src/Linux/script/Ubuntu-22.04-Official.sh)修改而成

> [!CAUTION]
> 本腳本會修改核心模組，執行前請確認已備份重要資料，並在測試環境驗證後再用於生產環境。

### 環境需求

- ExHyper-V
- OS：fnOS(1.1.23)
- Kernel：`6.12.18`
- 依賴套件：`dkms`、`wget`、`linux-source-6.12`
- WSL2-Linux-Kernel 原始碼（位於 `/tmp/WSL2-Linux-Kernel`）

### 主要修改內容

| 步驟 | 說明 |
|------|------|
| 複製標頭 | 從 WSL2-Linux-Kernel 提取 `d3dkmthk.h`、`hyperv.h`、`eventfd.h` |
| 原始碼調整 | 修正 `hyperv.h` 引用、`eventfd_signal()` API 相容性 |
| 下載補充標頭 | 取得 `extra-defines.h`（來源：MBRjun/dxgkrnl-dkms-lts）|
| DKMS 設定 | 自動產生 `dkms.conf` 並完成編譯安裝 |

### 測試環境

- Windows版本：Windows 10 工作站專業版 Build.19045
- Hyper-V 版本：10.0.19041.1
- ExHyperV版本：V1.4.2
- Host GPU：RTX 3060Ti
- fnOS 版本：1.1.23

### 測試結果

- ✅ `dkms status` 顯示模組已安裝
- ✅ `lsmod | grep dxgkrnl` 模組已載入
- ✅ Vulkan	
- ✅ Codec	
- ✅ CUDA/OpenCL

### 參考資料

- [ExHyperV Ubuntu-22.04-Official.sh](https://github.com/Justsenger/ExHyperV/blob/main/src/Linux/script/Ubuntu-22.04-Official.sh)
- [staralt/dxgkrnl-dkms](https://github.com/staralt/dxgkrnl-dkms)
- [MBRjun/dxgkrnl-dkms-lts](https://github.com/MBRjun/dxgkrnl-dkms-lts)

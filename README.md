## Development
To build your own firmware you need a GNU/Linux, BSD or macOS system (case sensitive filesystem required). Cygwin is unsupported because of the lack of a case sensitive file system.<br/>

  ### Requirements
  To build with this project, Debian 11 is preferred. And you need use the CPU based on AMD64 architecture, with at least 4GB RAM and 25 GB available disk space. Make sure the __Internet__ is accessible.

  The following tools are needed to compile ImmortalWrt, the package names vary between distributions.

  - Here is an example for Debian/Ubuntu users:<br/>
    - Method 1:
      <details>
        <summary>Setup dependencies via APT</summary>

        ```bash
        sudo apt update -y
        sudo apt full-upgrade -y
        sudo apt install -y ack antlr3 asciidoc autoconf automake autopoint binutils bison build-essential \
          bzip2 ccache clang cmake cpio curl device-tree-compiler ecj fastjar flex gawk gettext gcc-multilib \
          g++-multilib git gnutls-dev gperf haveged help2man intltool lib32gcc-s1 libc6-dev-i386 libelf-dev \
          libglib2.0-dev libgmp3-dev libltdl-dev libmpc-dev libmpfr-dev libncurses-dev libpython3-dev \
          libreadline-dev libssl-dev libtool libyaml-dev libz-dev lld llvm lrzsz mkisofs msmtp nano \
          ninja-build p7zip p7zip-full patch pkgconf python3 python3-pip python3-ply python3-docutils \
          python3-pyelftools qemu-utils re2c rsync scons squashfs-tools subversion swig texinfo uglifyjs \
          upx-ucl unzip vim wget xmlto xxd zlib1g-dev zstd
        ```
      </details>
    - Method 2:
      ```bash
      sudo bash -c 'bash <(curl -s https://build-scripts.immortalwrt.org/init_build_environment.sh)'
      ```

  Note:
  - Do everything as an unprivileged user, not root, without sudo.
  - Using CPUs based on other architectures should be fine to compile ImmortalWrt, but more hacks are needed - No warranty at all.
  - You must __not__ have spaces or non-ascii characters in PATH or in the work folders on the drive.
  - If you're using Windows Subsystem for Linux (or WSL), removing Windows folders from PATH is required, please see [Build system setup WSL](https://openwrt.org/docs/guide-developer/build-system/wsl) documentation.
  - Using macOS as the host build OS is __not__ recommended. No warranty at all. You can get tips from [Build system setup macOS](https://openwrt.org/docs/guide-developer/build-system/buildroot.exigence.macosx) documentation.
  - For more details, please see [Build system setup](https://openwrt.org/docs/guide-developer/build-system/install-buildsystem) documentation.

  ### Quickstart
  1. Run `git clone -b openwrt-24.10-6.6 --single-branch --filter=blob:none https://github.com/RuijieNetworksCommunity/immortalwrt-mt798x-24.10-6.6 mt798x-24.10` to clone the source code.
  2. Run `cd mt798x-24.10` to enter source directory.
  3. Run `./scripts/feeds update -a` to obtain all the latest package definitions defined in feeds.conf / feeds.conf.default
  4. Run `./scripts/feeds install -a` to install symlinks for all obtained packages into package/feeds/
  5. Copy the configuration file for your device from the `defconfig` directory to the project root directory and rename it `.config`
     
     ```
     # MT7981
     cp -f defconfig/mt7981-ax3000.config .config

     # MT7986
     cp -f defconfig/mt7986-ax6000.config .config
     
  6. Run `make` to build your firmware. This will download all sources, build the cross-compile toolchain and then cross-compile the GNU/Linux kernel & all chosen applications for your target system.

  ### Related Repositories
  The main repository uses multiple sub-repositories to manage packages of different categories. All packages are installed via the OpenWrt package manager called opkg. If you're looking to develop the web interface or port packages to ImmortalWrt, please find the fitting repository below.
  - [LuCI Web Interface](https://github.com/RuijieNetworksCommunity/luci): Modern and modular interface to control the device via a web browser.
  - [ImmortalWrt Packages](https://github.com/RuijieNetworksCommunity/packages): Community repository of ported packages.
  - [OpenWrt Routing](https://github.com/openwrt/routing): Packages specifically focused on (mesh) routing.
  - [OpenWrt Video](https://github.com/openwrt/video): Packages specifically focused on display servers and clients (Xorg and Wayland).

## License
ImmortalWrt is licensed under [GPL-2.0-only](https://spdx.org/licenses/GPL-2.0-only.html).

## Filogic 设备型号速查

以下列表根据 [RuijieNetworksCommunity 的 filogic.mk](https://github.com/RuijieNetworksCommunity/MT798X-6.6-24.10/blob/openwrt-24.10-6.6/target/linux/mediatek/image/filogic.mk) 中的全部 `TARGET_DEVICES` 整理。共 149 个构建目标。添加设备时，将表格最后一列对应行复制到 `.config`，并确认设备所需 DTS、分区布局及依赖包与当前源码匹配。

```text
CONFIG_TARGET_mediatek=y
CONFIG_TARGET_mediatek_filogic=y
CONFIG_TARGET_mediatek_filogic_DEVICE_<设备ID>=y
```

| 厂商 | 型号 / 变体 | 设备 ID | 可复制的配置字段 |
|---|---|---|---|
| Ruijie | RG-X60 Pro (Stock) | `ruijie-rg-x60-pro-stock` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie-rg-x60-pro-stock=y` |
| Ruijie | RG-X60 Pro (107m) | `ruijie-rg-x60-pro` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie-rg-x60-pro=y` |
| Ruijie | EW-6000GX Pro (Stock) | `ruijie_ew-6000gx-pro-stock` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_ew-6000gx-pro-stock=y` |
| Ruijie | EW-6000GX Pro (107m) | `ruijie_ew-6000gx-pro` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_ew-6000gx-pro=y` |
| Ruijie | EW-6000GX Pro (OpenWrt U-Boot layout) | `ruijie_ew-6000gx-pro-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_ew-6000gx-pro-ubootmod=y` |
| Ruijie | RG-X60 (Stock) | `ruijie-rg-x60-stock` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie-rg-x60-stock=y` |
| Ruijie | RG-X60 (107m) | `ruijie-rg-x60` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie-rg-x60=y` |
| Ruijie | RG-X30E (stock layout) | `ruijie_rg-x30e-stock` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_rg-x30e-stock=y` |
| Ruijie | RG-X30E (firmware2 layout) | `ruijie_rg-x30e-firmware2` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_rg-x30e-firmware2=y` |
| Ruijie | RG-X30E | `ruijie_rg-x30e` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_rg-x30e=y` |
| Ruijie | RG-X30E Pro (stock layout) | `ruijie_rg-x30e-pro-stock` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_rg-x30e-pro-stock=y` |
| Ruijie | RG-X30E Pro (firmware2 layout) | `ruijie_rg-x30e-pro-firmware2` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_rg-x30e-pro-firmware2=y` |
| Ruijie | RG-X30E Pro | `ruijie_rg-x30e-pro` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ruijie_rg-x30e-pro=y` |
| ABT | ASR3000 | `abt_asr3000` | `CONFIG_TARGET_mediatek_filogic_DEVICE_abt_asr3000=y` |
| Acelink | EW-7886CAX | `acelink_ew-7886cax` | `CONFIG_TARGET_mediatek_filogic_DEVICE_acelink_ew-7886cax=y` |
| Acer | Predator Connect W6 | `acer_predator-w6` | `CONFIG_TARGET_mediatek_filogic_DEVICE_acer_predator-w6=y` |
| Acer | Predator Connect W6d | `acer_predator-w6d` | `CONFIG_TARGET_mediatek_filogic_DEVICE_acer_predator-w6d=y` |
| Acer | Connect Vero W6m | `acer_vero-w6m` | `CONFIG_TARGET_mediatek_filogic_DEVICE_acer_vero-w6m=y` |
| — | SDG-8612 | `smartrg_sdg-8612` | `CONFIG_TARGET_mediatek_filogic_DEVICE_smartrg_sdg-8612=y` |
| — | SDG-8614 | `smartrg_sdg-8614` | `CONFIG_TARGET_mediatek_filogic_DEVICE_smartrg_sdg-8614=y` |
| — | SDG-8622 | `smartrg_sdg-8622` | `CONFIG_TARGET_mediatek_filogic_DEVICE_smartrg_sdg-8622=y` |
| — | SDG-8632 | `smartrg_sdg-8632` | `CONFIG_TARGET_mediatek_filogic_DEVICE_smartrg_sdg-8632=y` |
| — | SDG-8733 | `smartrg_sdg-8733` | `CONFIG_TARGET_mediatek_filogic_DEVICE_smartrg_sdg-8733=y` |
| — | SDG-8733A | `smartrg_sdg-8733a` | `CONFIG_TARGET_mediatek_filogic_DEVICE_smartrg_sdg-8733a=y` |
| — | SDG-8734 | `smartrg_sdg-8734` | `CONFIG_TARGET_mediatek_filogic_DEVICE_smartrg_sdg-8734=y` |
| ASUS | RT-AX52 | `asus_rt-ax52` | `CONFIG_TARGET_mediatek_filogic_DEVICE_asus_rt-ax52=y` |
| ASUS | RT-AX59U | `asus_rt-ax59u` | `CONFIG_TARGET_mediatek_filogic_DEVICE_asus_rt-ax59u=y` |
| ASUS | TUF-AX4200 | `asus_tuf-ax4200` | `CONFIG_TARGET_mediatek_filogic_DEVICE_asus_tuf-ax4200=y` |
| Arcadyan | Mozart | `arcadyan_mozart` | `CONFIG_TARGET_mediatek_filogic_DEVICE_arcadyan_mozart=y` |
| ASUS | TUF-AX6000 | `asus_tuf-ax6000` | `CONFIG_TARGET_mediatek_filogic_DEVICE_asus_tuf-ax6000=y` |
| Bananapi | BPi-R3 | `bananapi_bpi-r3` | `CONFIG_TARGET_mediatek_filogic_DEVICE_bananapi_bpi-r3=y` |
| Bananapi | BPi-R3 Mini | `bananapi_bpi-r3-mini` | `CONFIG_TARGET_mediatek_filogic_DEVICE_bananapi_bpi-r3-mini=y` |
| — | BPi-R4 | `bananapi_bpi-r4` | `CONFIG_TARGET_mediatek_filogic_DEVICE_bananapi_bpi-r4=y` |
| — | BPi-R4 2.5GE | `bananapi_bpi-r4-poe` | `CONFIG_TARGET_mediatek_filogic_DEVICE_bananapi_bpi-r4-poe=y` |
| Cetron | CT3003 (stock layout) | `cetron_ct3003` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cetron_ct3003=y` |
| Cetron | CT3003 (OpenWrt U-Boot layout) | `cetron_ct3003-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cetron_ct3003-ubootmod=y` |
| CMCC | A10 | `cmcc_a10` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_a10=y` |
| CMCC | A10 (stock layout) | `cmcc_a10-stock` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_a10-stock=y` |
| CMCC | A10 (OpenWrt U-Boot layout) | `cmcc_a10-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_a10-ubootmod=y` |
| Philips | HY3000 | `philips_hy3000` | `CONFIG_TARGET_mediatek_filogic_DEVICE_philips_hy3000=y` |
| CMCC | RAX3000M EMMC (MTK layout) | `cmcc_rax3000m-emmc-mtk` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_rax3000m-emmc-mtk=y` |
| CMCC | RAX3000M NAND (MTK layout) | `cmcc_rax3000m-nand-mtk` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_rax3000m-nand-mtk=y` |
| CMCC | RAX3000M NAND | `cmcc_rax3000m` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_rax3000m=y` |
| CMCC | RAX3000Me | `cmcc_rax3000me` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_rax3000me=y` |
| CMCC | RAX3000Z (eMMC version) | `cmcc_rax3000z-emmc` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_rax3000z-emmc=y` |
| CMCC | XR30 NAND (U-Boot mod) | `cmcc_xr30-nand-mod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_xr30-nand-mod=y` |
| CMCC | XR30 EMMC (U-Boot mod) | `cmcc_xr30-emmc-mod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_xr30-emmc-mod=y` |
| CMCC | XR30 NAND (H layout) | `cmcc_xr30-stock` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cmcc_xr30-stock=y` |
| CLX | s20l (eMMC version) | `clx_s20l` | `CONFIG_TARGET_mediatek_filogic_DEVICE_clx_s20l=y` |
| CLX | s20p (eMMC version) | `clx_s20p` | `CONFIG_TARGET_mediatek_filogic_DEVICE_clx_s20p=y` |
| COMFAST | CF-E393AX | `comfast_cf-e393ax` | `CONFIG_TARGET_mediatek_filogic_DEVICE_comfast_cf-e393ax=y` |
| Confiabits | MT7981 | `confiabits_mt7981` | `CONFIG_TARGET_mediatek_filogic_DEVICE_confiabits_mt7981=y` |
| Cudy | AP3000 Outdoor v1 | `cudy_ap3000outdoor-v1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_ap3000outdoor-v1=y` |
| Cudy | AP3000 v1 | `cudy_ap3000-v1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_ap3000-v1=y` |
| Cudy | M3000 v1 | `cudy_m3000-v1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_m3000-v1=y` |
| Cudy | RE3000 v1 | `cudy_re3000-v1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_re3000-v1=y` |
| Cudy | TR3000 v1 | `cudy_tr3000-v1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_tr3000-v1=y` |
| Cudy | TR3000 v1 MOD | `cudy_tr3000-v1-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_tr3000-v1-ubootmod=y` |
| Cudy | TR3000 v1 256mb | `cudy_tr3000-v1-256mb` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_tr3000-v1-256mb=y` |
| Cudy | WR3000 v1 | `cudy_wr3000-v1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_wr3000-v1=y` |
| Cudy | WR3000S v1 | `cudy_wr3000s-v1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_wr3000s-v1=y` |
| Cudy | WBR3000UAX v1 | `cudy_wbr3000uax-v1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_wbr3000uax-v1=y` |
| Cudy | WBR3000UAX v1 (OpenWrt U-Boot layout) | `cudy_wbr3000uax-v1-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_cudy_wbr3000uax-v1-ubootmod=y` |
| D-Link | AQUILA PRO AI M30 A1 | `dlink_aquila-pro-ai-m30-a1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_dlink_aquila-pro-ai-m30-a1=y` |
| D-Link | AQUILA PRO AI M60 A1 | `dlink_aquila-pro-ai-m60-a1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_dlink_aquila-pro-ai-m60-a1=y` |
| Edgecore | EAP111 | `edgecore_eap111` | `CONFIG_TARGET_mediatek_filogic_DEVICE_edgecore_eap111=y` |
| GatoNetworks | gdsp | `gatonetworks_gdsp` | `CONFIG_TARGET_mediatek_filogic_DEVICE_gatonetworks_gdsp=y` |
| GL.iNet | GL-MT2500 | `glinet_gl-mt2500` | `CONFIG_TARGET_mediatek_filogic_DEVICE_glinet_gl-mt2500=y` |
| GL.iNet | GL-MT3000 | `glinet_gl-mt3000` | `CONFIG_TARGET_mediatek_filogic_DEVICE_glinet_gl-mt3000=y` |
| GL.iNet | GL-MT6000 | `glinet_gl-mt6000` | `CONFIG_TARGET_mediatek_filogic_DEVICE_glinet_gl-mt6000=y` |
| — | GL-X3000 | `glinet_gl-x3000` | `CONFIG_TARGET_mediatek_filogic_DEVICE_glinet_gl-x3000=y` |
| — | GL-XE3000 | `glinet_gl-xe3000` | `CONFIG_TARGET_mediatek_filogic_DEVICE_glinet_gl-xe3000=y` |
| OpenFi | 6C (with 256MB+1GB+M.2 for 5G+U3) | `openfi_6c` | `CONFIG_TARGET_mediatek_filogic_DEVICE_openfi_6c=y` |
| H3C | Magic NX30 Pro (OpenWrt U-Boot layout) | `h3c_magic-nx30-pro` | `CONFIG_TARGET_mediatek_filogic_DEVICE_h3c_magic-nx30-pro=y` |
| H3C | Magic NX30 Pro (NMBM layout) | `h3c_magic-nx30-pro-nmbm` | `CONFIG_TARGET_mediatek_filogic_DEVICE_h3c_magic-nx30-pro-nmbm=y` |
| Huasifei | WH3000 Pro | `huasifei_wh3000-pro` | `CONFIG_TARGET_mediatek_filogic_DEVICE_huasifei_wh3000-pro=y` |
| Airpi | emmc | `Airpi` | `CONFIG_TARGET_mediatek_filogic_DEVICE_Airpi=y` |
| Airpi | AP3000M | `airpi_ap3000m` | `CONFIG_TARGET_mediatek_filogic_DEVICE_airpi_ap3000m=y` |
| Huasifei | WH3000 eMMC | `huasifei_wh3000-emmc` | `CONFIG_TARGET_mediatek_filogic_DEVICE_huasifei_wh3000-emmc=y` |
| Imou | LC-HX3001 | `imou_lc-hx3001` | `CONFIG_TARGET_mediatek_filogic_DEVICE_imou_lc-hx3001=y` |
| JCG | Q30 PRO | `jcg_q30-pro` | `CONFIG_TARGET_mediatek_filogic_DEVICE_jcg_q30-pro=y` |
| JDCloud | RE-CP-03 | `jdcloud_re-cp-03` | `CONFIG_TARGET_mediatek_filogic_DEVICE_jdcloud_re-cp-03=y` |
| Keenetic | KN-3811 | `keenetic_kn-3811` | `CONFIG_TARGET_mediatek_filogic_DEVICE_keenetic_kn-3811=y` |
| Keenetic | KN-3911 | `keenetic_kn-3911` | `CONFIG_TARGET_mediatek_filogic_DEVICE_keenetic_kn-3911=y` |
| Konka | KOMI A31 | `konka_komi-a31` | `CONFIG_TARGET_mediatek_filogic_DEVICE_konka_komi-a31=y` |
| Livinet | ZR-3020 (stock layout) | `livinet_zr-3020` | `CONFIG_TARGET_mediatek_filogic_DEVICE_livinet_zr-3020=y` |
| Livinet | ZR-3020 (OpenWrt U-Boot layout) | `livinet_zr-3020-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_livinet_zr-3020-ubootmod=y` |
| MediaTek | 7981r128 | `mediatek_7981r128` | `CONFIG_TARGET_mediatek_filogic_DEVICE_mediatek_7981r128=y` |
| MediaTek | MT7981 rfb | `mediatek_mt7981-rfb` | `CONFIG_TARGET_mediatek_filogic_DEVICE_mediatek_mt7981-rfb=y` |
| MediaTek | MT7986 rfba AP (NAND) | `mediatek_mt7986a-rfb-nand` | `CONFIG_TARGET_mediatek_filogic_DEVICE_mediatek_mt7986a-rfb-nand=y` |
| MediaTek | MTK7986 rfbb AP | `mediatek_mt7986b-rfb` | `CONFIG_TARGET_mediatek_filogic_DEVICE_mediatek_mt7986b-rfb=y` |
| MediaTek | MT7988A rfb | `mediatek_mt7988a-rfb` | `CONFIG_TARGET_mediatek_filogic_DEVICE_mediatek_mt7988a-rfb=y` |
| MERCUSYS | MR80X v3 | `mercusys_mr80x-v3` | `CONFIG_TARGET_mediatek_filogic_DEVICE_mercusys_mr80x-v3=y` |
| MERCUSYS | MR90X v1 | `mercusys_mr90x-v1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_mercusys_mr90x-v1=y` |
| MERCUSYS | MR90X v1 (UBI) | `mercusys_mr90x-v1-ubi` | `CONFIG_TARGET_mediatek_filogic_DEVICE_mercusys_mr90x-v1-ubi=y` |
| Netcore | N60 | `netcore_n60` | `CONFIG_TARGET_mediatek_filogic_DEVICE_netcore_n60=y` |
| Netcore | N60 Pro | `netcore_n60-pro` | `CONFIG_TARGET_mediatek_filogic_DEVICE_netcore_n60-pro=y` |
| NETGEAR | WAX220 | `netgear_wax220` | `CONFIG_TARGET_mediatek_filogic_DEVICE_netgear_wax220=y` |
| netis | NX31 | `netis_nx31` | `CONFIG_TARGET_mediatek_filogic_DEVICE_netis_nx31=y` |
| Netis | NX32U | `netis_nx32u` | `CONFIG_TARGET_mediatek_filogic_DEVICE_netis_nx32u=y` |
| Nokia | EA0326GMP | `nokia_ea0326gmp` | `CONFIG_TARGET_mediatek_filogic_DEVICE_nokia_ea0326gmp=y` |
| OpenEmbed | SOM7981 | `openembed_som7981` | `CONFIG_TARGET_mediatek_filogic_DEVICE_openembed_som7981=y` |
| OpenWrt | One | `openwrt_one` | `CONFIG_TARGET_mediatek_filogic_DEVICE_openwrt_one=y` |
| Qihoo | 360T7 | `qihoo_360t7` | `CONFIG_TARGET_mediatek_filogic_DEVICE_qihoo_360t7=y` |
| Routerich | AX3000 | `routerich_ax3000` | `CONFIG_TARGET_mediatek_filogic_DEVICE_routerich_ax3000=y` |
| Routerich | AX3000 (OpenWrt U-Boot layout) | `routerich_ax3000-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_routerich_ax3000-ubootmod=y` |
| Routerich | AX3000 v1 | `routerich_ax3000-v1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_routerich_ax3000-v1=y` |
| DaZoo | yvr-x6 (ax6000) | `yvr_x6` | `CONFIG_TARGET_mediatek_filogic_DEVICE_yvr_x6=y` |
| DaZoo | yvr-x6 (ax6000 512M) | `yvr_x6_512M` | `CONFIG_TARGET_mediatek_filogic_DEVICE_yvr_x6_512M=y` |
| SL | 3000 eMMC | `sl_3000-emmc` | `CONFIG_TARGET_mediatek_filogic_DEVICE_sl_3000-emmc=y` |
| SN | R1 | `sn_r1` | `CONFIG_TARGET_mediatek_filogic_DEVICE_sn_r1=y` |
| BT | BT-R320 | `bt_r320` | `CONFIG_TARGET_mediatek_filogic_DEVICE_bt_r320=y` |
| BT | BT-RB300 | `bt_rb300` | `CONFIG_TARGET_mediatek_filogic_DEVICE_bt_rb300=y` |
| SuperElectron | ZN-M5 (stock layout) | `superelectron_zn-m5-stock` | `CONFIG_TARGET_mediatek_filogic_DEVICE_superelectron_zn-m5-stock=y` |
| TP-Link | RE6000XD | `tplink_re6000xd` | `CONFIG_TARGET_mediatek_filogic_DEVICE_tplink_re6000xd=y` |
| — | TL-XDR4288 | `tplink_tl-xdr4288` | `CONFIG_TARGET_mediatek_filogic_DEVICE_tplink_tl-xdr4288=y` |
| — | TL-XDR6086 | `tplink_tl-xdr6086` | `CONFIG_TARGET_mediatek_filogic_DEVICE_tplink_tl-xdr6086=y` |
| — | TL-XDR6088 | `tplink_tl-xdr6088` | `CONFIG_TARGET_mediatek_filogic_DEVICE_tplink_tl-xdr6088=y` |
| — | TL-XTR8488 | `tplink_tl-xtr8488` | `CONFIG_TARGET_mediatek_filogic_DEVICE_tplink_tl-xtr8488=y` |
| TP-Link | WMA301 | `tplink_wma301` | `CONFIG_TARGET_mediatek_filogic_DEVICE_tplink_wma301=y` |
| TP-Link | WMA301 (stock layout) | `tplink_wma301-stock` | `CONFIG_TARGET_mediatek_filogic_DEVICE_tplink_wma301-stock=y` |
| TP-Link | WMA301 (OpenWrt layout) | `tplink_wma301-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_tplink_wma301-ubootmod=y` |
| Ubiquiti | UniFi U6+ | `ubnt_unifi-6-plus` | `CONFIG_TARGET_mediatek_filogic_DEVICE_ubnt_unifi-6-plus=y` |
| UMI | UAX3000E | `umi_uax3000e` | `CONFIG_TARGET_mediatek_filogic_DEVICE_umi_uax3000e=y` |
| — | unielec_u7981-01-emmc (EMMC) | `unielec_u7981-01-emmc` | `CONFIG_TARGET_mediatek_filogic_DEVICE_unielec_u7981-01-emmc=y` |
| — | unielec_u7981-01-nand (NAND) | `unielec_u7981-01-nand` | `CONFIG_TARGET_mediatek_filogic_DEVICE_unielec_u7981-01-nand=y` |
| WAVLINK | WL-WN586X3 | `wavlink_wl-wn586x3` | `CONFIG_TARGET_mediatek_filogic_DEVICE_wavlink_wl-wn586x3=y` |
| WAVLINK | WL-WN573HX3 | `wavlink_wl-wn573hx3` | `CONFIG_TARGET_mediatek_filogic_DEVICE_wavlink_wl-wn573hx3=y` |
| Xiaomi | Mi Router AX3000T (stock layout) | `xiaomi_mi-router-ax3000t` | `CONFIG_TARGET_mediatek_filogic_DEVICE_xiaomi_mi-router-ax3000t=y` |
| Xiaomi | Mi Router AX3000T (MTK U-Boot layout) | `xiaomi_mi-router-ax3000t-mtkuboot` | `CONFIG_TARGET_mediatek_filogic_DEVICE_xiaomi_mi-router-ax3000t-mtkuboot=y` |
| Xiaomi | Mi Router AX3000T (OpenWrt U-Boot layout) | `xiaomi_mi-router-ax3000t-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_xiaomi_mi-router-ax3000t-ubootmod=y` |
| Xiaomi | Mi Router WR30U (stock layout) | `xiaomi_mi-router-wr30u-stock` | `CONFIG_TARGET_mediatek_filogic_DEVICE_xiaomi_mi-router-wr30u-stock=y` |
| Xiaomi | Mi Router WR30U | `xiaomi_mi-router-wr30u-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_xiaomi_mi-router-wr30u-ubootmod=y` |
| Xiaomi | Redmi Router AX6000 (stock layout) | `xiaomi_redmi-router-ax6000-stock` | `CONFIG_TARGET_mediatek_filogic_DEVICE_xiaomi_redmi-router-ax6000-stock=y` |
| Xiaomi | Redmi Router AX6000 (OpenWrt U-Boot layout) | `xiaomi_redmi-router-ax6000-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_xiaomi_redmi-router-ax6000-ubootmod=y` |
| Xiaomi | Redmi Router AX6000 (U-Boot mod) | `xiaomi_redmi-router-ax6000-mod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_xiaomi_redmi-router-ax6000-mod=y` |
| YunCore | AX835 | `yuncore_ax835` | `CONFIG_TARGET_mediatek_filogic_DEVICE_yuncore_ax835=y` |
| Zbtlink | ZBT-Z8102AX | `zbtlink_zbt-z8102ax` | `CONFIG_TARGET_mediatek_filogic_DEVICE_zbtlink_zbt-z8102ax=y` |
| Zbtlink | ZBT-Z8103AX | `zbtlink_zbt-z8103ax` | `CONFIG_TARGET_mediatek_filogic_DEVICE_zbtlink_zbt-z8103ax=y` |
| Zbtlink | ZBT-Z8103AX-C | `zbtlink_zbt-z8103ax-c` | `CONFIG_TARGET_mediatek_filogic_DEVICE_zbtlink_zbt-z8103ax-c=y` |
| Zbtlink | ZBT-Z8103AX-C (NMBM) | `zbtlink_zbt-z8103ax-c-nmbm` | `CONFIG_TARGET_mediatek_filogic_DEVICE_zbtlink_zbt-z8103ax-c-nmbm=y` |
| Zbtlink | ZBT-Z8103AX-C (OpenWrt U-Boot layout) | `zbtlink_zbt-z8103ax-c-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_zbtlink_zbt-z8103ax-c-ubootmod=y` |
| Zyxel | EX5601-T0 (stock layout) | `zyxel_ex5601-t0-stock` | `CONFIG_TARGET_mediatek_filogic_DEVICE_zyxel_ex5601-t0-stock=y` |
| Zyxel | EX5601-T0 (OpenWrt U-Boot layout) | `zyxel_ex5601-t0-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_zyxel_ex5601-t0-ubootmod=y` |
| Zyxel | EX5700 (Telenor) | `zyxel_ex5700-telenor` | `CONFIG_TARGET_mediatek_filogic_DEVICE_zyxel_ex5700-telenor=y` |
| Zyxel | NWA50AX Pro | `zyxel_nwa50ax-pro` | `CONFIG_TARGET_mediatek_filogic_DEVICE_zyxel_nwa50ax-pro=y` |
| Wireless-Tag | ZX7981PG | `wirelesstag_zx7981pg` | `CONFIG_TARGET_mediatek_filogic_DEVICE_wirelesstag_zx7981pg=y` |
| Wireless-Tag | ZX7981PD | `wirelesstag_zx7981pd` | `CONFIG_TARGET_mediatek_filogic_DEVICE_wirelesstag_zx7981pd=y` |
| Wireless-Tag | ZX7981PD (OpenWrt U-Boot layout) | `wirelesstag_zx7981pd-ubootmod` | `CONFIG_TARGET_mediatek_filogic_DEVICE_wirelesstag_zx7981pd-ubootmod=y` |

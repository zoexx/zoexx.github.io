title: 认识 LKM
date: 2018-03-13 10:25:15
tags: linux
---

> 首先什么是内核模块呢?这对于初学者无非是个非常难以理解的概念。内核模块是Linux内核向外部提供的一个插口，其全称为动态可加载内核模块（Loadable Kernel Module，LKM），我们简称为模块。Linux内核之所以提供模块机制，是因为它本身是一个单内核（monolithic kernel）。单内核的最大优点是效率高，因为所有的内容都集成在一起，但其缺点是可扩展性和可维护性相对较差，模块机制就是为了弥补这一缺陷。


#### 一个初始化的阿里云ECS centos 7.4 64位 系统都有哪些模块呢

|Module                 | Size |  Used by|中文注释|
|------------------------|-----|-------------|------|
|sb_edac               | 32018 |  0 |       纠错 使用校验码解决电磁干扰问题
|edac_core             | 58151 |  1 sb_edac|纠错 使用校验码解决电磁干扰问题
|iosf_mbi              | 13523 |  0 |
|crc32_pclmul          | 13113 |  0 |
|ghash_clmulni_intel   | 13259 |  0 |
|aesni_intel           | 69884 |  0 |
|lrw                   | 13286 |  1 aesni_intel|
|gf128mul              | 14951 |  1 lrw|
|glue_helper           | 13990 |  1 aesni_intel|
|ablk_helper           | 13597 |  1 aesni_intel|
|ppdev                 | 17671 |  0 |
|cryptd                | 20359 |  3 ghash_clmulni_intel,aesni_intel,ablk_helper|
|sg                    | 40721 |  0 |
|virtio_balloon        | 13864 |  0 |
|joydev                | 17377 |  0 |
|pcspkr                | 12718 |  0 |
|i2c_piix4             | 22390 |  0 |
|parport_pc            | 28165 |  0 |
|parport               | 42299 |  2 ppdev,parport_pc|
|ip_tables             | 27115 |  0 |
|ext4                 | 571503 |  1 | 文件系统 |
|mbcache               | 14958 |  1 ext4|
|jbd2                 | 103046 |  1 ext4|
|virtio_console        | 28066 |  1 |
|virtio_net            | 28096 |  0 |
|virtio_blk            | 18156 |  2 |
|sr_mod                | 22416 |  0 |
|cdrom                 | 42556 |  1 sr_mod|
|ata_generic           | 12910 |  0 |
|pata_acpi             | 13038 |  0 |
|cirrus                | 24515 |  1 |
|drm_kms_helper       | 159169 |  1 cirrus|
|syscopyarea           | 12529 |  1 drm_kms_helper|
|sysfillrect           | 12701 |  1 drm_kms_helper|
|sysimgblt             | 12640 |  1 drm_kms_helper|
|fb_sys_fops           | 12703 |  1 drm_kms_helper|
|ttm                   | 99345 |  1 cirrus|
|drm                  | 370825 |  4 ttm,drm_kms_helper,cirrus|
|ata_piix              | 35038 |  0 |
|libata               | 238896 |  3 pata_acpi,ata_generic,ata_piix|
|crct10dif_pclmul      | 14289 |  0 |
|crct10dif_common      | 12595 |  1 crct10dif_pclmul|
|crc32c_intel          | 22079 |  0 |
|serio_raw             | 13413 |  0 |
|virtio_pci            | 22913 |  0 |
|virtio_ring           | 22746 |  5 virtio_blk,virtio_net,virtio_pci,virtio_balloon,virtio_console|
|virtio                | 14959 |  5 virtio_blk,virtio_net,virtio_pci,virtio_balloon,virtio_console|
|i2c_core              | 40756 |  3 drm,i2c_piix4,drm_kms_helper|
|floppy                | 69417 |  0 |



- 未完待续
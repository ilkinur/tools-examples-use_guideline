JFFS2 filesystem extraction tool

Features

    big-endian and little-endian support with auto-detection
    zlib, rtime, LZMA, and LZO compression support
    CRC checks - for now only enforced on hdr_crc
    extraction of symlinks, directories, files, and device nodes
    detection/handling of duplicate inode numbers. Occurs if multiple JFFS2 filesystems are found in one file and causes jefferson to treat segments as separate filesystems

Usage

$ jefferson filesystem.img -d outdir

source: https://github.com/sviehb/jefferson
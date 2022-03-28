## 名称

readelf - 显示ELF 文件信息

## 概要

> ```
> readelf [-a|--all]
>      [-h|--file-header]
>      [-l|--program-headers|--segments]
>      [-S|--section-headers|--sections]
>      [-g|--section-groups]
>      [-t|--section-details]
>      [-e|--headers]
>      [-s|--syms|--symbols]
>      [--dyn-syms]
>      [-n|--notes]
>      [-r|--relocs]
>      [-u|--unwind]
>      [-d|--dynamic]
>      [-V|--version-info]
>      [-A|--arch-specific]
>      [-D|--use-dynamic]
>      [-x <number or name>|--hex-dump=<number or name>]
>      [-p <number or name>|--string-dump=<number or name>]
>      [-R <number or name>|--relocated-dump=<number or name>]
>      [-z|--decompress]
>      [-c|--archive-index]
>      [-w[lLiaprmfFsoRtUuTgAckK]|
>       --debug-dump[=rawline,=decodedline,=info,=abbrev,=pubnames,=aranges,=macro,=frames,=frames-interp,=str,=loc,=Ranges,=pubtypes,=trace_info,=trace_abbrev,=trace_aranges,=gdb_index,=addr,=cu_index,=links,=follow-links]]
>      [--dwarf-depth=n]
>      [--dwarf-start=n]
>      [-I|--histogram]
>      [-v|--version]
>      [-W|--wide]
>      [-H|--help]
>      elffile...
> ```

## 描述

`readelf` 用于显示一个或多个ELF 格式 obj 文件的信息，选项用于控制显示什么信息。

`elffile...`  是用于显示的obj 文件。

## 选项

| 选项 | 选项                              | 解释                                                         |
| ---- | --------------------------------- | ------------------------------------------------------------ |
| -a   | --all                             | 等同于同时指定，--file-header, --program-headers, --sections, --symbols, --relocs, --dynamic, --notes, --version-info, --arch-specific, --unwind,--section-groups 和 --histogram。<br />注意，该选项没有使能`--use-dynamic` ，所以如果没有额外指定该选项， 动态符号表和动态重定向表的信息不会显示。 |
| -h   | --file-header                     | 显示文件开始的ELF 头部信息                                   |
| -l   | --program-header<br />--segments  | 显示文件`segment`头部信息(如果存在)                          |
| -S   | --sections<br />--settion-headers | 显示文件`setcion` 头信息                                     |
| -g   | --section-groups                  | 显示文件`section` 组信息                                     |
| -t   | --setcion-details                 | 显示详细的`section` 信息                                     |
| -s   | --symbols<br />--syms             |                                                              |




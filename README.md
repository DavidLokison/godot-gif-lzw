# Godot GIF LZW
GIF's LZW compression/decompression done in Godot. Whole code is based on GIF specification and [this website](http://www.matthewflickinger.com/lab/whatsinagif/bits_and_bytes.asp).

## Installation and Usage
Add this repository as a submodule via `git submodule add http://DavidLokison/godot-gif-lzw gif-lzw`. Make sure to `git submodule update --init --recursive` afterwards.

Then, load and instanciate the script and use it as follows:

### Compression
If you want to compress an image, use `compress_lzw(image: PackedByteArray, colors: PackedByteArray) -> Array`.

```gdscript
extends Node2D

var lzw := preload("res://gif-lzw/lzw.gd").new()

func _ready():
    var image: PackedByteArray = [
        1, 1, 1, 1, 1, 2, 2, 2, 2, 2,
        1, 1, 1, 1, 1, 2, 2, 2, 2, 2,
        1, 1, 1, 1, 1, 2, 2, 2, 2, 2,
        1, 1, 1, 0, 0, 0, 0, 2, 2, 2,
        1, 1, 1, 0, 0, 0, 0, 2, 2, 2,
        2, 2, 2, 0, 0, 0, 0, 1, 1, 1,
        2, 2, 2, 0, 0, 0, 0, 1, 1, 1,
        2, 2, 2, 2, 2, 1, 1, 1, 1, 1,
        2, 2, 2, 2, 2, 1, 1, 1, 1, 1,
        2, 2, 2, 2, 2, 1, 1, 1, 1, 1,
    ]

    var color_table := PackedByteArray([0, 1, 2, 3])

    var compressed_res: Array = lzw.compress_lzw(image, color_table)
    var compressed_data: PackedByteArray = compressed_res[0]
    var min_code_size: int = compressed_res[1]
```

### Decompression
If you want to decompress an image, use `decompress_lzw(code_stream_data: PackedByteArray, min_code_size: int, colors: PackedByteArray) -> PackedByteArray`.

```gdscript
extends Node2D

var lzw := preload("res://gif-lzw/lzw.gd").new()

func _ready():
    var image: PackedByteArray = [
        1, 1, 1, 1, 1, 2, 2, 2, 2, 2,
        1, 1, 1, 1, 1, 2, 2, 2, 2, 2,
        1, 1, 1, 1, 1, 2, 2, 2, 2, 2,
        1, 1, 1, 0, 0, 0, 0, 2, 2, 2,
        1, 1, 1, 0, 0, 0, 0, 2, 2, 2,
        2, 2, 2, 0, 0, 0, 0, 1, 1, 1,
        2, 2, 2, 0, 0, 0, 0, 1, 1, 1,
        2, 2, 2, 2, 2, 1, 1, 1, 1, 1,
        2, 2, 2, 2, 2, 1, 1, 1, 1, 1,
        2, 2, 2, 2, 2, 1, 1, 1, 1, 1,
    ]

    var color_table := PackedByteArray([0, 1, 2, 3])

    var compressed_res: Array = lzw.compress_lzw(image, color_table)
    var compressed_data: PackedByteArray = compressed_res[0]
    var min_code_size: int = compressed_res[1]

    var decompressed_index_stream: PackedByteArray = lzw.decompress_lzw(
            compressed_data,
            min_code_size,
            color_table)
```

## Credits
Credits for the original algorithm all goes to jegor377 in his [original repository](https://github.com/jegor377/godot-gif-lzw). This repository merely updates to Godot 4.3 stable and refactors to be used as a submodule.

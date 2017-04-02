+++
date = "2017-03-29T22:42:54+05:30"
draft = false
title = "golang在windows下读取clipboard截取图片信息"
+++

### golang二进制处理

使用'encoding/binary'包，和bytes.Buffer配置。

```golang
import "encoding/binary"

data := new(bytes.Buffer)
binary.Write(data, binary.LittleEndian, uint16('B')|(uint16('M')<<8))
```

### golang从windows下的clipboard中抓取图片

windows下的clipboard保存的图片信息是dib结构，保存有bmpinfoheader信息头和pixel图片信息。

其中bmpinfoheader信息格式如下:
```golang
type infoHeader struct {
    iSize          uint32 // infoheader 长度，40
    iWidth         uint32 // 图片宽度
    iHeight        uint32 // 图片高度
    iPLanes        uint16 
    iBitCount      uint16 // 每个像素颜色占用空间
    iCompression   uint32 // 图片压缩格式，dib中一般为0或者为3
    iSizeImage     uint32 // 图片pixel占用大小
    iXPelsPerMeter uint32
    iYPelsPerMeter uint32
    iClrUsed       uint32
    iClrImportant  uint32
}
```

需要根据bmpinfoheader计算出bmp图片的fileheader，并放在bmp图片的头部。fileheader格式如下:
```golang
type fileHeader struct {
    bfType      uint16 // 恒定为'BM'
    bfSize      uint32 // bmp整个文件大小
    bfReserved1 uint16 
    bfReserved2 uint16
    bfOffBits   uint32 // pixel信息开始的偏移量
}
```

读取clipboard中图片信息，需要用到win32的api，如下:
```golang
var (
    user32                     = syscall.MustLoadDLL("user32")
    openClipboard              = user32.MustFindProc("OpenClipboard")
    closeClipboard             = user32.MustFindProc("CloseClipboard")
    emptyClipboard             = user32.MustFindProc("EmptyClipboard")
    getClipboardData           = user32.MustFindProc("GetClipboardData")
    setClipboardData           = user32.MustFindProc("SetClipboardData")
    isClipboardFormatAvailable = user32.MustFindProc("IsClipboardFormatAvailable")

    kernel32     = syscall.NewLazyDLL("kernel32")
    globalAlloc  = kernel32.NewProc("GlobalAlloc")
    globalFree   = kernel32.NewProc("GlobalFree")
    globalLock   = kernel32.NewProc("GlobalLock")
    globalUnlock = kernel32.NewProc("GlobalUnlock")
    lstrcpy      = kernel32.NewProc("lstrcpyW")
    copyMemory   = kernel32.NewProc("CopyMemory")
)
```

由于bmp图片信息为压缩，比较占用空间，一般保存到电脑上，可以使用jpg格式，golang bmp转jpg格式非常简单, 需要使用到"golang.org/x/image/bmp"
和"image/jpeg"包

```golang
    original_image, err := bmp.Decode(dat)
    if err != nil {
        log.Fatal(err)
    }

    err = jpeg.Encode(f, original_image, nil) // f为二进制写权限打开的文件句柄
    if err != nil {
        log.Fatal(err)
    }
```

[代码](https://github.com/robinchenyu/clipboard_go/blob/master/main.go)




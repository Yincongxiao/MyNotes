##YYDiskCache
* 获取系统文件控件剩余空间,其中`NSFileSystemFreeSize`代表剩余空间,`NSFileSystemSize`代表全部空间,`NSFileSystemNodes`代表文件个数
```objc
/// Free disk space in bytes.
static int64_t _YYDiskSpaceFree() {
    NSError *error = nil;
    NSDictionary *attrs = [[NSFileManager defaultManager] attributesOfFileSystemForPath:NSHomeDirectory() error:&error];
    if (error) return -1;
    int64_t space =  [[attrs objectForKey:NSFileSystemFreeSize] longLongValue];
    if (space < 0) space = -1;
    return space;
}
```
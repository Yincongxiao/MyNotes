##YYImage学习
* 计算一个UIImage在内存中占用的空间:

```objc
- (NSUInteger)imageCost:(UIImage *)image {
    CGImageRef cgImage = image.CGImage;
    if (!cgImage) return 1;
    //取出图片的高度
    CGFloat height = CGImageGetHeight(cgImage);
    //取出每行所占用的bytes值
    size_t bytesPerRow = CGImageGetBytesPerRow(cgImage);
    //做乘法
    NSUInteger cost = bytesPerRow * height;
    if (cost == 0) cost = 1;
    return cost;
}
```
#####iOS 中利用 NSMutableAttributedString 实现富文本排版
* 下面是利用 `NSMutableAttributedString` 实现富文本排版
```objc
/*
     // Predefined character attributes for text. If the key is not in the dictionary, then use the default values as described below.
     UIKIT_EXTERN NSString * const NSFontAttributeName NS_AVAILABLE(10_0, 6_0);                // 字体 UIFont, default Helvetica(Neue) 12
     UIKIT_EXTERN NSString * const NSParagraphStyleAttributeName NS_AVAILABLE(10_0, 6_0);      // 段洛样式 NSParagraphStyle, default defaultParagraphStyle
     UIKIT_EXTERN NSString * const NSForegroundColorAttributeName NS_AVAILABLE(10_0, 6_0);     // 字体颜色 UIColor, default blackColor
     UIKIT_EXTERN NSString * const NSBackgroundColorAttributeName NS_AVAILABLE(10_0, 6_0);     // 背景色 UIColor, default nil: no background
     UIKIT_EXTERN NSString * const NSLigatureAttributeName NS_AVAILABLE(10_0, 6_0);            // 字母连接符(艺术字) NSNumber containing integer, default 1: default ligatures, 0: no ligatures
     UIKIT_EXTERN NSString * const NSKernAttributeName NS_AVAILABLE(10_0, 6_0);                // 字间距 NSNumber containing floating point value, in points; amount to modify default kerning. 0 means kerning is disabled.
     UIKIT_EXTERN NSString * const NSStrikethroughStyleAttributeName NS_AVAILABLE(10_0, 6_0);  // 删除线 NSNumber containing integer, default 0: no strikethrough
     UIKIT_EXTERN NSString * const NSUnderlineStyleAttributeName NS_AVAILABLE(10_0, 6_0);      // 下划线 NSNumber containing integer, default 0: no underline
     UIKIT_EXTERN NSString * const NSStrokeColorAttributeName NS_AVAILABLE(10_0, 6_0);         // 字体填充颜色 UIColor, default nil: same as foreground color
     UIKIT_EXTERN NSString * const NSStrokeWidthAttributeName NS_AVAILABLE(10_0, 6_0);         // 字画宽度 NSNumber containing floating point value, in percent of font point size, default 0: no stroke; positive for stroke alone, negative for stroke and fill (a typical value for outlined text would be 3.0)
     UIKIT_EXTERN NSString * const NSShadowAttributeName NS_AVAILABLE(10_0, 6_0);              // 阴影 NSShadow, default nil: no shadow
     UIKIT_EXTERN NSString *const NSTextEffectAttributeName NS_AVAILABLE(10_10, 7_0);          // 设置文本特殊效果，取值为 NSString 对象，目前只有图版印刷效果可用  NSString, default nil: no text effect
     
     UIKIT_EXTERN NSString * const NSAttachmentAttributeName NS_AVAILABLE(10_0, 7_0);          // 文本中附件,用于图文混排 NSTextAttachment, default nil
     UIKIT_EXTERN NSString * const NSLinkAttributeName NS_AVAILABLE(10_0, 7_0);                // 文中链接 NSURL (preferred) or NSString
     UIKIT_EXTERN NSString * const NSBaselineOffsetAttributeName NS_AVAILABLE(10_0, 7_0);      // 调整字体基线偏移量,正值向上偏移,负值向下偏移 NSNumber containing floating point value, in points; offset from baseline, default 0
     UIKIT_EXTERN NSString * const NSUnderlineColorAttributeName NS_AVAILABLE(10_0, 7_0);      // 下划线颜色 UIColor, default nil: same as foreground color
     UIKIT_EXTERN NSString * const NSStrikethroughColorAttributeName NS_AVAILABLE(10_0, 7_0);  // 字体边缘颜色 UIColor, default nil: same as foreground color
     UIKIT_EXTERN NSString * const NSObliquenessAttributeName NS_AVAILABLE(10_0, 7_0);         // 字体斜度 NSNumber containing floating point value; skew to be applied to glyphs, default 0: no skew
     UIKIT_EXTERN NSString * const NSExpansionAttributeName NS_AVAILABLE(10_0, 7_0);           // 设置字体扁平化效果 NSNumber containing floating point value; log of expansion factor to be applied
     */
    
    UITextView *textLabel = [[UITextView alloc] initWithFrame:CGRectMake(10, 100, [UIScreen mainScreen].bounds.size.width - 20, 0)];
    
    NSString *title = @"Two pages of the tax return were revealed by US TV network MSNBC, triggering an angry response from the White House.\n It said publishing the tax return was illegal.\n Mr Trump refused to release his tax returns during the election campaign, breaking with a long-held tradition. \nThe two pages were a fraction of the complete return and did not include details of Mr Trump's income. \nCorrespondents say the leak is still significant because so little is known about President Trump's tax returns and the new information will increase pressure on him to release more. \nThe two pages show that Mr Trump paid $5.3m in federal income tax and an extra $31m in what is called alternative minimum tax (AMT).";
    NSMutableParagraphStyle *style = [NSMutableParagraphStyle new];
    style.lineBreakMode = NSLineBreakByClipping;
    style.lineSpacing = 1.0f;
    style.paragraphSpacing = 20.0f;
    
    NSMutableAttributedString *attString = [[NSMutableAttributedString alloc] initWithString:title attributes:@{
                                                                                                                NSFontAttributeName : [UIFont fontWithName:@"Zapfino" size:12],
                                                                                                                NSParagraphStyleAttributeName : style,
                                                                                                                NSForegroundColorAttributeName : [UIColor whiteColor],
                                                                                                                NSBackgroundColorAttributeName : [UIColor blackColor],
                                                                                                                NSLigatureAttributeName : @(1),
                                                                                                                NSKernAttributeName : @(1),
                                                                                                                }];

    [attString addAttribute:NSStrikethroughStyleAttributeName value:@(2) range:NSMakeRange(2, 2)];
    [attString addAttribute:NSUnderlineStyleAttributeName value:@(2) range:NSRangeFromString(@"安静")];
    textLabel.attributedText = attString;
    [textLabel sizeToFit];
    [self.view addSubview:textLabel];
    
    self.view.backgroundColor = [UIColor lightGrayColor];
    
    NSArray *famliyNames = [UIFont familyNames];
    
    int i = 0;
    while (i < famliyNames.count - 1) {
        i ++;
        NSLog(@"系统所有支持字体名字: %@",famliyNames[i]);
    }
```
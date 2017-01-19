---
title: iOS-TableView
date: 2016-12-28 09:53:25
tags:
- iOS
- TableView
- 笔记
---
## TableView
TableView是一种仅可在竖直方向上滚动的列表视图，是ScrollView的子类。
### Table View Styles and Accessory Views
#### Table View Styles
TableVie 有两种style:  plain and grouped. 区别在于外观。
##### Plain Table Views
Plain Table View 是常用的样式。它的每一个section都可以有header和footer。当用户滑动到特定的sectio 时，其header会浮动在 table view 的头部。（*footer会浮动到底部。待验证*）
![Plain Table View](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/TableView_iPhone/Art/tv_plain_style.jpg)
 
还有一种常见的样式Plain Table View 是 indexed list。此索引列表提供了一种快捷定位到具体section的能力。
![indexed list](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/TableView_iPhone/Art/tv_indexed_list.jpg) 

##### Grouped Table Views
Grouped Table View 将关系紧密的rows分到一组。iOS系统设置页面就是此种样式的典型应用。
![Grouped Table Views](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/TableView_iPhone/Art/tv_grouped_style.jpg)

#### Standard Styles for Table View Cells
系统提供了四种样式的Cell。当然你也可以自定义TableViewCell。

* [UITableViewCellStyleDefault](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/TableView_iPhone/Art/tvcellstyle_default.jpg)
* [UITableViewCellStyleSubtitle](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/TableView_iPhone/Art/tvcellstyle_subtitle.jpg) 
* [UITableViewCellStyleValue1](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/TableView_iPhone/Art/tvcellstyle_value1.jpg) 
* [UITableViewCellStyleValue2](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/TableView_iPhone/Art/tvcellstyle_value2.jpg)
 
#### Accessory Views
系统提供了三种样式的accessory view。当然你也可以自定义accessory view。

[详情](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/TableView_iPhone/TableViewStyles/TableViewCharacteristics.html#//apple_ref/doc/uid/TP40007451-CH3-SW2)

### TableViewCell 
#### 利用缓存池来重用cell优化性能
重用需注意cell状态的重置，避免视图与数据对应不上。换言之，可能前面的Cell A选中了，滑动时，新进入屏幕的cell重用了Cell A，并且cell仍然被选中。

#### Cell的高度
##### 固定高度
对于定高需求的表格，强烈建议使用这种方式保证不必要的高度计算和调用
	
	self.tableView.rowHeight = 88;
另一种方式就是实现 UITableViewDelegate 中的：

	- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath {
	    // return xxx
	}
需要注意的是，实现了这个方法后，rowHeight 的设置将无效。所以，这个方法适用于具有多种 cell 高度的 UITableView。

##### 动态高度

## 参考链接

[Table View Programming Guide for iOS](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/TableView_iPhone/AboutTableViewsiPhone/AboutTableViewsiPhone.html#//apple_ref/doc/uid/TP40007451)
[iOS开发系列--UITableView全面解析](http://www.cnblogs.com/kenshincui/p/3931948.html#introduction)
[优化UITableViewCell高度计算的那些事](http://blog.sunnyxx.com/2015/05/17/cell-height-calculation/)
[Using Auto Layout in UITableView for dynamic cell layouts & variable row heights](http://stackoverflow.com/questions/18746929/using-auto-layout-in-uitableview-for-dynamic-cell-layouts-variable-row-heights)
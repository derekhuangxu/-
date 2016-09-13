# iOS开发琐碎知识点整理
### 2016.9
#### 9.6
在tableview使用group分类的时候，控制header和footer的大小使用下面的代码

	//section头部高度
	- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section
	{
	return CGFLOAT_MIN;
	}
	//section头部视图
	- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section
	{
	UIView *view=[\[UIView alloc]() initWithFrame:CGRectMake(0, 0, self.view.width, CGFLOAT_MIN)];
	view.backgroundColor = [UIColor clearColor]();
	   return view ;
	}
	//section底部间距
	- (CGFloat)tableView:(UITableView *)tableView heightForFooterInSection:(NSInteger)section
	{
	return 12.f;
	}
	//section底部视图
	- (UIView *)tableView:(UITableView *)tableView viewForFooterInSection:(NSInteger)section
	{
	UIView *view=[\[UIView alloc]() initWithFrame:CGRectMake(0, 0, self.view.width, 12.f)];
	view.backgroundColor = [UIColor clearColor]();
	return view ;
	}

####9.7
在reload某一个row的时候，会发生contentoffset变化的bug，一下几个方法可以解决
	
	
	1.可以尝试使用reload
	2.可以尝试下提前记录offSet
	GPoint offset = tableView.contentOffset;
	- (void)reloadSections:(NSIndexSet *)sections withRowAnimation:(UITableViewRowAnimation)animation; //用UITableViewRowAnimationNone
	[tableView layoutIfNeeded]; // 强制更新
	[tableView setContentOffset:offset]
	3.刷新之前更改estimatedRowHeight
	-(void)keepTableviewContentOffSetCell:(CellClass *)cell{
	   CGRect rect = cell.frame;
	   self.tableView.estimatedRowHeight = rect.size.height;
	}
####9.8
1)假如你使用极光推送没有声音，可以查看下你推送过来的sound字段，假如系统是iOS8之后的，并且有sound字段而且返回为空字符串，你可以试试返回sound字段为1107试试，这样就能播放系统的声音了。


2)在应用程序中，可以使用修饰符来改变局部变量和实例变量的语义

	__strong：默认语义，保留此值。
	__unsafe_unretained：不保留辞职，这么做可能不安全，因为等到再次使用变量时候，其对象可能已经回收了。
	__weak：不保留此值，但是变量可以安全使用，因为如果系统把这个对象回收了，那么变量也会自动清空。
	__autoreleasing：把对象“按引用传递”（pass by reference）给方法时，使用这个特殊的修饰符，此值在方法返回时自动释放。


####9.9
在适配UILabel自动换行的时候，我们会用到下面这个方法;

	- (CGRect)boundingRectWithSize:(CGSize)size options:(NSStringDrawingOptions)options attributes:(nullable NSDictionary<NSString *, id> *)attributes context:(nullable NSStringDrawingContext *)context NS_AVAILABLE(10_11, 7_0);
	
但是，有时候，会遇到计算的高度不正确的时候，假如遇到了，可以尝试检查下面两个问题：
	
	1、NSStringDrawingOptions应该有两个：NSStringDrawingUsesLineFragmentOrigin 以及 NSStringDrawingUsesFontLeading
	2、这个方法返回值为CGRect，在使用此计算的高度时候，要用这个方法：ceilf(rect.size.height)。

假如你想省事，可以使用我简单封装后的这个方法：
	
	/*!
	 *
	 *  @brief 返回lable自适应高度
	 *
	 *  @param textStr label所展示字符串
 	 *  @param font    label字体大小
   	 *  @param spacing label距离两侧距离
 	 *
 	 *  @return label自适应字符串的高度
 	 *
 	 */
	+ (CGFloat)lableHeightWithTextStr:(NSString *)textStr fontSize:(CGFloat)font labelSpacing:(CGFloat)spacing {
   
	  	CGFloat lblTextMaxWidth  = [UIApplication sharedApplication].keyWindow.width - spacing;
	  	CGSize lblTextMaxSize    = CGSizeMake(lblTextMaxWidth, MAXFLOAT);
	   	NSDictionary * attribute = @{NSFontAttributeName :[UIFont systemFontOfSize:font]};
	  	CGRect rect = [textStr boundingRectWithSize:lblTextMaxSize
                                	        options:NSStringDrawingUsesLineFragmentOrigin | NSStringDrawingUsesFontLeading
                                	   attributes:attribute
                                	   context:nil];
	 	return ceilf(rect.size.height);
	}


####9.13
假如你遇到了这种

	Undefined symbols for architecture arm64:
	
你可以看看自己需要用到的静态库是不是都添加进了工程。


设置`UISearchController`取消按钮的颜色

    searchController.searchBar.tintColor = [UIColor colorWithHexString:@"ff5500"];


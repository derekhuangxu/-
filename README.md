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

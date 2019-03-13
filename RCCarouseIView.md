#pragma mark - RCCarouselViewDelegate
- (NSInteger)numberOfItemsInCarouselView {
    return self.recommendCountryList.count;
}

- (RCCarouselViewItem *)carouselView:(RCCarouselView *)carouselView itemForRowAtIndex:(int)index {
    RCCarouselViewItem *item = [[RCCarouselViewItem alloc] initWithFrame:CGRectMake(0, 0, HeaderViewImageSize.width, HeaderViewImageSize.height)];
    item.layer.cornerRadius = 5.0f;
    item.layer.masksToBounds = YES;
    item.orderNumber = index;
    
    GHAllCountryListDetailModel *detailModel = [self getRangeDataArrayByCountryList][index];
    UIImageView *imageView = [[UIImageView alloc] initWithFrame:item.bounds];
    [imageView gh_setImageWithURL:detailModel.imageUrl placeholderImage:[UIImage imageNamed:@""]];
    imageView.userInteractionEnabled = NO;
    imageView.contentMode = UIViewContentModeScaleAspectFill;
    [item addSubview:imageView];
    
    CAGradientLayer *centerMaskLayer = [CAGradientLayer layer];
    [centerMaskLayer setColors:@[(__bridge id)[UIColor colorWithRed:11.f/255.0 green:11.f/255.0 blue:11.f/255.0 alpha:0].CGColor,
                                 (__bridge id)[UIColor colorWithRed:11.f/255.0 green:11.f/255.0 blue:11.f/255.0 alpha:0.36*0.5].CGColor,
                                 (__bridge id)[UIColor colorWithRed:0.f/255.0 green:0.f/255.0 blue:0.f/255.0 alpha:0.66*0.5].CGColor,
                                 (__bridge id)[UIColor colorWithRed:28.f/255.0 green:28.f/255.0 blue:28.f/255.0 alpha:1*0.5].CGColor,
                                 ]];
    centerMaskLayer.startPoint = CGPointMake(0.5, 0);
    centerMaskLayer.endPoint = CGPointMake(0.5, 1);
    centerMaskLayer.locations = @[@(0), @(0.3f), @(0.6f), @(1.0f)];
    centerMaskLayer.frame = CGRectMake(0, HeaderViewImageSize.height/2, HeaderViewImageSize.width, HeaderViewImageSize.height/2);
    [item.layer addSublayer:centerMaskLayer];
    
    UILabel *countryLabel = [[UILabel alloc] init];
    countryLabel.textColor = [GHColor gh_textColorTextWhite];
    countryLabel.font = [UIFont systemFontOfSize:15 weight:UIFontWeightMedium];
    countryLabel.textAlignment = NSTextAlignmentCenter;
    countryLabel.frame = CGRectMake(0, HeaderViewImageSize.height - 27, HeaderViewImageSize.width, 15);
    countryLabel.text = detailModel.name;
    countryLabel.userInteractionEnabled = NO;
    [item addSubview:countryLabel];

   
    return item;
}


- (void)carouselView:(RCCarouselView *)carouselView didSelectItemAtIndex:(int)index {
    GHAllCountryListDetailModel *detailModel = [self getRangeDataArrayByCountryList][index];
    GHCountryVC *countryVC = [[GHCountryVC alloc] init];
    countryVC.countryID = detailModel.countryId;
    [self.navigationController pushViewController:countryVC animated:YES];
}

- (void)carouselView:(RCCarouselView *)carouselView willDisplayItemforIndex:(NSInteger)index {
    GHLogDebug(@"currentIndex: %d", self.carouselView.currentIndex);
//    if (index < self.carouselView.itemArray.count) {
//        [self refreshCarouselViewMaskLayerAndLabel];
//    }
}

- (NSArray *)getRangeDataArrayByCountryList {
    NSMutableArray *dataArray = [NSMutableArray array];
    for (NSInteger i = _recommendCountryList.count/2 + 1; i < _recommendCountryList.count; i ++) {
        [dataArray addObject:_recommendCountryList[i]];
    }
    for (NSInteger i = 0; i < _recommendCountryList.count/2 + 1; i++) {
        [dataArray addObject:_recommendCountryList[i]];
    }
    return dataArray;
}

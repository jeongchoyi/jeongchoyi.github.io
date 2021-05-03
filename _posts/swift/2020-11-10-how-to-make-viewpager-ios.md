---
layout: post
title: "iOS ViewPager 구현하기" 
date: 2020-11-10
category: read 
excerpt: ""

---

# iOS ViewPager 구현하기

<img src="https://user-images.githubusercontent.com/28949235/98568141-1d11c300-22f4-11eb-8c55-5aca6507803f.gif" alt="RPReplay_Final1604939239" width=400px />

이런 식으로 작동하는 ViewPager(in Android...)를 구현해 봅시다!  
iOS에서는 이 기능을 기본으로 지원해주지 않기 때문에 구현하는 방법이 정말정말 여러가지 방법이 있더라구요 ...  
그래서 저도 당근마켓 클론코딩 스터디 몇 주차에 걸쳐 여러 방법으로 구현해 봤는데

1. UISegmentedControl custom하기
2. [XLPagerTabStrip](https://github.com/xmartlabs/XLPagerTabStrip) 라이브러리 사용하기
3. UICollectionView 기반
4. UIPageViewController 기반

이번엔 4번의 방법으로 구현한 내용을 정리해 보려고 합니당












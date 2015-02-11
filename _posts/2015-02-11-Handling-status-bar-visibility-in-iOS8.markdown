---
layout: post
title:  "Handling Status bar visibility on IOS 8"
date:   2015-02-11 13:46:48
categories: ios
---

A recent issue I faced on an iOS app was the status bar getting hidden on landscape (on the iPhone). Debugging on XCode 6.1 with 
    
    po [[UIApplication sharedApplication] statusBar]

gave the following results:

Portrait
    
    <UIStatusBar: 0x7897be00; frame = (0 0; 320 20); opaque = NO; autoresize = W+BM; layer = <CALayer: 0x786d2110>>

Landscape

	<UIStatusBar: 0x7897be00; frame = (0 0; 568 20); alpha = 0; hidden = YES; opaque = NO; autoresize = W+BM; layer = <CALayer: 0x786d2110>>

Nowhere in my code did I state that the status bar should be hidden, and yet the ```hidden = YES``` value was set. My fix was to implement
```(BOOL)prefersStatusBarHidden``` and return ```NO``` in the top ViewController as

	- (BOOL)prefersStatusBarHidden
	{
		return NO;
	}

This solved the issue and the status bar was back! Apple documentation states that if unimplemented it, the method defaults to no. However, somewhere along the view stack it got set to ```YES``` in my app. Each rotation calls the ```prefersStatusBarHidden``` method. Till I can trace back the root cause, this is the solution I have to go with.

[jekyll]:      http://jekyllrb.com
[jekyll-quickstart]: http://jekyllrb.com/docs/quickstart/
[github.io]: https://pages.github.com/
[page-link]: https://raw.githubusercontent.com/absessive/absessive.github.io/master/_posts/2015-02-11-welcome-to-jekyll.markdown
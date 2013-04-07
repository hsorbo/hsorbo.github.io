---
layout: post
title: "Sigslots and @selectors"
description: ""
category: 
comments: false
tags: []
---
{% include JB/setup %}

I've been hacking libjingle on mac lately. Libjingle uses the sigslots library for callbacks/events/signals in C++. Objective-C/Cocoa offers the same functionality+++ through it's selectors, and even though Objective-C supports C++, aka Objective-C++, it's limited. You can't have an NSObject derived class using templates as needed by sigslots. Here comes a few dirty C++ lines which plumbs sigslots to objective-c selectors.

Btw. I haven't looked into getting well formatted code in my blog posts, sorry for the inconvenience.

	template <typename T>
	class SignalToSelector : public sigslot::has_slots<> {
	public:
	SignalToSelector(SEL selector, NSObject *target){
		_selector = selector;
		_target = target;
	}

	void OnSignal(T type) {
		if (_target != NULL && _target != nil && [_target respondsToSelector:_selector])
		[_target performSelector:_selector withObject: [[SignalWrapperObject alloc] initWithObject: (void *) &type]];
	}
	private:
	SEL _selector;
	NSObject *_target;
	};

This is just a quick and dirty prototype and only works with signal1, but implementing singal2-6 should be easy. You should also do some releasing. The SignalWrapperObject is a NSObject taking a void pointer to the c++ object. The reasen for the wrapping is that withObject: is of type id, which in turn has to be an NSObject or a subclass thereof. You can of course edit the underlying c++ code to use NSObejcts using ObjC++, in my case this is a third party lib and I don't want to make non-generic modifications to it.
Here is an example of plumbing a sigslot to a selector

	/* Create the wrapper, point it to the mehtod test: on self */
	SignalToSelector<buzz::XmppEngine::State> test(@selector(test:), self);
	
	/* Bind the sigslot to the objects OnSignal method */
	pump.client()->SignalStateChange.connect(&test, &SignalToSelector<buzz::XmppEngine::State>::OnSignal);

here is the test: method.

	-(void) test: (id) dataobject
	{
		/* we like to cast */
		buzz::XmppEngine::State *state = (buzz::XmppEngine::State *)[(SignalWrapperObject *)dataobject getObject];
		/* do something with state */
	}


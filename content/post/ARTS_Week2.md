---
title:  "Arts Week 2"
date:   2019-10-18
categories: ["arts"]
---

## Algorithm

编程珠玑第一章 disk sort

100000个不重复电话号码, 在有限的内存空间里, 要求排序。

{% highlight python %}
#!/usr/bin/env python
BISTSPERWORD = 32
SHIFT = 5
MASK = ord('\x1F')
N = 10000

# Setting all bits to 0
a = [0 for x in range(N)]

def set(i):
  a[i>>SHIFT] |=  (1<<(i & MASK))

def clear(i):
  a[i>>SHIFT] &= ~(1<<(i & MASK))

def test(i):
  return (a[i>>SHIFT] & (1<<(i & MASK))) != 0

# testing
set(12)
set(15)

#print MASK
print test(11)
print test(15)
print test(88)
print test(12)

print '10 exists: ', test(10) # 10 not found
set(10)
print 'After set, 10 exists: ', test(10) # 10 found
{% endhighlight %}


The first 3 constants are inter-related. BITSPERWORD is 32.
This you'd want to set based on your compiler+architecture.
SHIFT is 5, because 2^5 = 32.

Finally, MASK is 0x1F which is 11111 in binary (ie: the bottom 5 bits are all set). Equivalently, MASK =  BITSPERWORD - 1.

The bitset is conceptually just an `array of bits`.

This implementation actually uses an array of ints, and assumes 32 bits per int.

So whenever we want to set, clear or test (read) a bit we need to figure out two things:
1. which int (of the array) is it in
2. which of that int's bits are we talking about

Because we're assuming 32 bits per int, we can just divide by 32 (and truncate) to get the array index we want.
Dividing by 32 (BITSPERWORD) is the same as shifting to the right by 5 (SHIFT).
So that's what the `a[i>>SHIFT]` bit is about.

You could also write this as `a[i/BITSPERWORD]`
Now that we know which element of a we want, we need to figure out which bit.

Really, we want the remainder. We could do this with `i%BITSPERWORD,` but it turns out that `i&MASK` is equivalent.

This is because BITSPERWORD is a power of 2 (2^5 in this case) and MASK is the bottom 5 bits all set.

`1<<(i & MASK)` returns the integer that only the reminder postion is set to 1.

This is the KEY, so that the `integer i is positioned correctly in array a`.


## Review:
Read Backbone Source Code

### How updating Backbone Model triggers View rendering?

Each Backbone View is a DOM element.

Backbone Model manages an internal table of data attributes, and triggers "change" events when any of its data is modified.

The flow is:
1. Set/Update model attributes

2. Set Model implementation fire ‘change’ event

3. The listener catches the event, and call the callback. Example: this.render()

4. render() use the template and updated model, re-generate the DOM element.

### Backbone Model Set():

Set a hash of model attributes on the object, firing `"change"`.
```
this.trigger('change', this, options);
```
This is the core primitive operation of a model, updating the data and notifying anyone who needs to know about the change in state.
```
// Triggered when a target did is added and the target is fetched.
this.listenTo(this.model, 'change', this.render);
this.listenTo(this.model, 'change:dids_details', this._updateCollection);
this.listenTo(this.model, 'change:primary_fax', this._updateCollection);
```
#### List of built-in Backbone events, with arguments:

https://backbonejs.org/#Events-catalog

Example:
```
"add" (model, collection, options) — when a model is added to a collection.

"remove" (model, collection, options) — when a model is removed from a collection.

"update" (collection, options) — single event triggered after any number of models have been added, removed or changed in a collection.

"reset" (collection, options) — when the collection's entire contents have been reset.

"sort" (collection, options) — when the collection has been re-sorted.

"change" (model, options) — when a model's attributes have changed.

"change:[attribute]" (model, value, options) — when a specific attribute has been updated.
```

https://backbonejs.org/#View-render

The ‘change’ event callback could call render() to re-generated the DOM element.

Example usage:
```
this.listenTo(this.model, 'change', this.render);
```
render() impl:
```
var Bookmark = Backbone.View.extend({
  template: _.template(...),
  render: function() {
    this.$el.html(this.template(this.model.attributes));
    return this;
  }
});
```

### What happened when you save a Backbone Model?
Ex:
```
this.model.save(saveOptions, {
      success: this._onSaved,
      error: this._onError
});
```

1. save() sends AJAX request to server.

2. The option.session callback is triggered after AJAX is done:
```
options.success = function(resp) {
  // Ensure attributes are restored during synchronous saves.
  model.attributes = attributes;
  var serverAttrs = model.parse(resp, options);
  if (options.wait) serverAttrs = _.extend(attrs || {}, serverAttrs);
  if (_.isObject(serverAttrs) && !model.set(serverAttrs, options)) {
     return false;
  }
  if (success) success(model, resp, options);
  model.trigger('sync', model, resp, options);
};
```

3. model.parse() will clear all existing attributes.

4. Call model.set() and pass in all current attributes from server.

5. Set() checks attributes from server. If response attributes are different from it previous have, update the model’s attributes. Trigger the ‘change’ event for all changed attributes.


### DOM Events vs Backbone Custom Events
backbone:listenTo() and Object.on('xx', function1(){}) linked with backbone trigger.

https://backbonejs.org/#Events-on

`events{} map` is DOM events. DOM events are registered in Jquery, so jquery dispatch it when DOM event triggers.

`Custom events` are done by backbone, use a list of events to maintain. Use `Observer pattern`

## Share
hacker and Painters - How to get wealth
measurement and impact是得到财富的两个关键要素，大公司大团队所得的measurement也是被整除了。
钱只是中间产物，创造财富就是满足他人的需要

## Tips
学习了mac 多桌面切换的快捷方式。

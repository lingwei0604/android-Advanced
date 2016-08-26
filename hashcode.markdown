hashcode的作用
--------

以java.lang.Object来理解，jvm 每 new一个Object，它都会将这个Object 丢到一个Hash哈希表中去，这样的话，，下次做object 的比较作者取这个对象的时候，它会根据对象的hashcode再从hash表中去这个对象，这样做的目的提高对象的效率，具体过程是这样：

        1：new Object（），jvm根据这个对象的hashcode值，放入到对应的hash表对应的key上，如果不同的对象确定产生了相同的hash值，也就是发生了hash key相同，导致冲突的情况，那么久在这个hash key 的地方产生一个链表，将所有产生相同的hashcode 的对象放到这个单链表上去，串在一起。
        
        2：比较两个对象的时候,首先根据他们的hashcode去hash表中找他的对象,当两个对象的hashcode相同,那么就是说他们这两个对象放在Hash表中的同一个key上,那么他们一定在这个key上的链表上。那么此时就只能根据Object的equal方法来比较这个对象是否equal。当两个对象的hashcode不同的话，肯定他们不能equal。
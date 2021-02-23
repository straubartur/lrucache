# lrucache
`
package main

import "fmt"

type Node struct {
	key string
    value string
    prev *Node
    next *Node

}
type LRUCache struct {
    head *Node
    tail *Node
    hash map[string]*Node
    capacity int
}

func Constructor(capacity int) LRUCache {
    hashmap := make(map[string]*Node)
    lruCache := LRUCache{
		nil,
		nil,
        hashmap,
        capacity,
    }
    return lruCache
}

func (lru *LRUCache) Get(key string) string {
	if lru.hash[key] != nil {
		prev := lru.hash[key].prev
		next := lru.hash[key].next
		lru.head.prev = lru.hash[key]
		lru.hash[key].next = lru.head
		lru.hash[key].prev = nil
		lru.head = lru.hash[key]
		if next != nil {
			next.prev = prev
		} else {
			lru.tail = prev
		}
		if prev != nil {
			prev.next = next
		}
		fmt.Println(lru.tail, lru.head)
		return lru.hash[key].value
	}
	return ""
}

func (lru *LRUCache) Put(key string, value string)  {
	fmt.Println(*lru, lru.hash)
    if lru.hash[key] != nil {
        lru.hash[key].value = value
        return
    }
    n := Node{
        key,
        value,
        nil,
        nil,
    }
    if lru.head == nil {
        lru.head = &n
        lru.tail = &n
    } else {
        if lru.capacity == len(lru.hash) {
            prev := lru.tail.prev
            prev.next = nil
            delete(lru.hash, lru.tail.key)
            lru.tail = prev
        }
        n.next = lru.head
		head := lru.head
		head.prev = &n
        lru.head = &n
    }
	lru.hash[key] = &n
}

func main() {
	lruCache := Constructor(5);
	lruCache.Put("artur", "1");
	lruCache.Put("artur2", "1");
	lruCache.Put("artur3", "1");
	lruCache.Put("artur4", "1");
	fmt.Println(lruCache.Get("artur"))
	fmt.Println(lruCache.Get("artur2"))
	fmt.Println(lruCache.Get("artur3"))
	fmt.Println(lruCache.Get("artur4"))
	fmt.Println(lruCache.Get("artur"))
}
`

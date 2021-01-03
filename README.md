
# SafeQueue

This class is a multi-producer, multi-consumer queue. It offers both blocking and non-blocking consuming as well, while producing is always blocking. The capacity is dynamically adjusted. The implementation is based on `std::queue`, and the thread safety is achieved using `std::mutex` and `std::condition_variable`.

---

The `Produce(T&& item)` function can be called with a rvalue reference. The `Consume(T& item)` function returns immediately with a boolean value, indicator of success. The `ConsumeSync(T& item)` method will block the calling thread until it can take an item from the queue. The `Finish` method stops all the waiting consumers (ConsumeSync), and make them return false.

## Example

Producer:

```c++
SafeQueue<MyType> queue;

// insert elements
queue.Produce(std::move(var1));
queue.Produce(std::move(var2));
```
Non-blocking consumer:

```c++
SafeQueue<MyEvent> event_queue;

// Game loop
while(!quit) {

	MyEvent event;
	while(event_queue.Consume(event)) {
		// Process the event
	}

	UpdateWorld();
	Render();
}
```

Blocking consumer:

```c++
SafeQueue<MyMessage> message_queue;

std::thread consumer([&](){
	MyMessage message;
	while(message_queue.ConsumeSync(message)) {
		// Process the message
	}
});

// ...
```

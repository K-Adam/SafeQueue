
# SafeQueue
This thread-safe queue implementation supports multiple providers and consumers. The `Provide(T&& item)` function can be called with a rvalue reference.  The `Consume(T& item)` function returns immediately with a boolean value, indicator of success. The `ConsumeSync(T& item)` method will block the calling thread until it can take an item from the queue. The `Finish` method stops all the waiting consumers (ConsumeSync), and make them return false.

## Example

Privider:

```c++
SafeQueue<MyType> queue;

// insert elements
queue.Provide(std::move(var1));
queue.Provide(std::move(var2));
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

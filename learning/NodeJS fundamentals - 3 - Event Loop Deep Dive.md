## Event Loop visualization

![](./images/1-event-loop.png)

### Execution steps: 
Within the event loop, the sequence of execution follows certain rules. There are quite a few rules to wrap your head around, so let's go over them one at a time:

1. Any callbacks in the microtask queue are executed. First, tasks in the nextTick queue and only then tasks in the promise queue.
2. All callbacks within the timer queue are executed.
3. Callbacks in the microtask queue (if present) are executed after every callback in the timer queue. First, tasks in the nextTick queue, and then tasks in the promise queue.
4. All callbacks within the I/O queue are executed.
5. Callbacks in the microtask queues (if present) are executed, starting with nextTickQueue and then Promise queue.
6. All callbacks in the check queue are executed.
7. Callbacks in the microtask queues (if present) are executed after every callback in the check queue. First, tasks in the nextTick queue, and then tasks in the promise queue.
8. All callbacks in the close queue are executed.
9. For one final time in the same loop, the microtask queues are executed. First, tasks in the nextTick queue, and then tasks in the promise queue.

## Reference: 
1. Vishwas Gopinath's article at - https://www.builder.io/blog/visual-guide-to-nodejs-event-loop
2. He also has a good youtube channel - https://www.youtube.com/@Codevolution
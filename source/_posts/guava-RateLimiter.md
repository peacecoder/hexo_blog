---
title: guava_RateLimiter
date: 2016-06-28 15:32:58
tags: guava, java
---


```java
package com.cmcm.news.meerkat.pns.pns;

import com.google.common.util.concurrent.RateLimiter;
import org.testng.annotations.Test;

import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.BlockingQueue;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class RateLimiterTest {
    private static final int STOP_FLAG = -1;
    public class Worker implements Runnable {
        private RateLimiter limiter;
        private BlockingQueue<Integer> queue;
        private int workerId;
        public Worker(int workerId, RateLimiter limiter, BlockingQueue<Integer> queue) {
            this.workerId = workerId;
            this.limiter = limiter;
            this.queue = queue;
        }

        @Override
        public void run() {
            while (true) {
                try {
                    limiter.acquire();
                    int value = this.queue.take();
                    if (value == STOP_FLAG) {
                        System.out.println("get stop flag...");
                        break;
                    }
                    System.out.println("Worker get " + value);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        }
    }

    @Test
    public void test() throws InterruptedException {
        int threadNum = 10;
        BlockingQueue<Integer> queue = new ArrayBlockingQueue<Integer>(100);
        RateLimiter limiter = RateLimiter.create(50);
        ExecutorService es = Executors.newFixedThreadPool(threadNum);
        for (int i = 0; i < threadNum; ++i) {
            es.submit(new Worker(i, limiter, queue));
        }
        long start = System.currentTimeMillis();
        for (int i = 0; i < 200; i++) {
            queue.put(i);
        }
        for (int i = 0; i < threadNum; ++i) {
            queue.put(STOP_FLAG);
        }
        while(!queue.isEmpty()) {
            Thread.sleep(10);
        }
        System.out.println("cost time: " + (System.currentTimeMillis() - start));
    }
}
```

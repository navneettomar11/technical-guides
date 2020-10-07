Code review and Unit testing are some of the best development pratices I always recommend, strive for, and enforce as much as possible. Even just by doing code review and Junit test case always offer postive result it can be improved a lot by constantly learning with our mistakes, other mistakes and by observing how others are doing it. I always try to get my code review done by some with more techinical and domain experience so that I can capture any domain-specific scenarios which have been missed during thing through the process.

I also try to get my code reviewed with someone with less experience so that I can improve the code readability, have a four-eye check and most importantly I found that when I explain my code to someone as part of code review I myself discover many things which can be improved or left out.

These are the things which I have been accumulated over the years but I also look forward to you guys to contribute your experience, best pratices for core review, and suggest how you guys do code review. These tips are independent of language and equally, apply to Java, NET or C++ code.

## 10 points checklist on Code Review
Here are some important points you can keep in mind while doing a code review for your team. This is not a formal checklist but something which I have created from my own experience. 

### 1. Functional Requirment
Does Code meet the **functional requirement**: first and foremost does code meets all requirements which it should meet, point out if anything has been left out.

### 2. Side effect on existing code
Is there any **Side effect of this change**: Sometimes one change in your system may cause a bug in other upstream and downstream system and it's quite possible that a new developer or anyone who is writing code might not be available on that dependency. This often directly related to experience in the project and I found that the more you know about the system and its environment better you able to figure this out.

### 3. Concurrency
Does code is thread-safe? Does it have properly synchronized if using the shared resources? Does it free of any kind of deadlock or live-lock? Concurrency bugs are hard to detect and often surfaces in production. Code review is one place where you can detect this by carefully understand design and its implementation.

### 4. Readability and maintainance
Does code is readable? Or is it too complicated for some-one completely new? Always give value to readability as code is not just for this time it will remain there for a long time and you need to read it many times.

Another important aspect is maintainance as most of the software spends 90% time on maintainance and only 10% time on development it should be maintainable and flexible in the first place.

You can verify whether code is configurable or not, look for any hard coding, find out what is going to be changed in the near future etc.

### 5. Consistency
This is part of point 4 but I have made it another separate point because of its importance. This the best thing you can have in your code with automatically achieves readability.

Since many developer and programmer take part in project and they have there own style of coding, it's in best interest of everybody to form a coding standard and follow it in letter and spirit.

For example it's not good someone using function initialize() and other is using init() for same kind of operation, keep you code consistent and it will look better, read better.

### 6. Performance
Another important aspect most important if you writing high volume low latency electronic trading platform for high frequency trading which strives for micro second latency. Carefully monitor which code is going to execute at start-up and which is going to be executed in a loop or multiple time optimize the code which is going to execute more often.

### 7. Error and Exception handling
Ask does code handles bad input and exception? It should and too with predefined and standard way which must be available and documented for support purpose. I put this point well above on my chart while doing a review because failling on this point can lead to your application crash and not able to recover from a fault on other systems or another part of the same application.

### 8. Simplicity
Always see if there is any simple and elegant alternative available at-least give a thought and try. Many times first solution comes in mind is not the best solution so giving another thought is just worth it.

### 9. Reuse of existing code
See if the functionaility can be achieved by using existing code, the advantge of doing this is that you are using tried and tested code which reduces your QA time and also gives you more confidence. Introducing new libraries introduce a new dependency. I prefer not try anything fancy until it's absolutely necessary.

### 10. Unit tests
Check whether enough JUnit test cases have been written and cover a sufficient percentage of new code let you pass the code without the JUnit test because the developer often makes an excuse of time but believe me its worth wrtinig it.





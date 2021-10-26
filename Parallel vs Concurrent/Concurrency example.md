# Concurrency Example: 

From: https://docs.microsoft.com/en-us/dotnet/csharp/programming-guide/concepts/async/

You'd write the instructions something like the following list to explain how to make a breakfast:

1.  Pour a cup of coffee.
2.  Heat up a pan, then fry two eggs.
3.  Fry three slices of bacon.
4.  Toast two pieces of bread.
5.  Add butter and jam to the toast.
6.  Pour a glass of orange juice.

If you have experience with cooking, you'd execute those instructions **asynchronously**. You'd start warming the pan for eggs, then start the bacon. You'd put the bread in the toaster, then start the eggs. At each step of the process, you'd start a task, then turn your attention to tasks that are ready for your attention.

Cooking breakfast is a good example of asynchronous work that isn't parallel. One person (or thread) can handle all these tasks. Continuing the breakfast analogy, one person can make breakfast asynchronously by starting the next task before the first completes. The cooking progresses whether or not someone is watching it. As soon as you start warming the pan for the eggs, you can begin frying the bacon. Once the bacon starts, you can put the bread into the toaster.

For a parallel algorithm, you'd need multiple cooks (or threads). One would make the eggs, one the bacon, and so on. Each one would be focused on just that one task. Each cook (or thread) would be blocked synchronously waiting for bacon to be ready to flip, or the toast to pop.
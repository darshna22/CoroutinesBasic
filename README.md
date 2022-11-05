# CoroutinesBasic
## Coroutine Scopes:
* __GlobalScope :__ Global scope is used to launch top-level coroutines which are operating on the whole application lifetime and are not cancelled prematurely
* __runBlocking :__  It will get run on the main thread,Runs a new coroutine and blocks the current thread until its completion.
* __viewModelScope :__ A ViewModelScope is defined for each ViewModel in your app. Any coroutine launched in this scope is automatically canceled if the ViewModel is cleared.
* __coroutineScope :__ CoroutineScope is a starting Point of Coroutine. CoroutineScope can have more than one coroutine within itself, which makes coroutine hierarchy. 
* __supervisiorScope:__ A supervisorScope won't cancel other children when one of them fails while CoroutineScope does.

## Coroutine Bulders:
* __launch :__ Can we use to execute any operation synchronously
* __Async  :__ Can we use to execute any operation aSynchronously
__Points to remembr:__
* It is known that async and launch are the two ways to start the coroutine. 
* Since It is known that async is used to get the result back, & should be used only when we need the parallel execution,
* whereas the launch is used when we do not want to get the result back and is used for the operation such as updating of data, etc.
* As we know that async is the only way till now to start the coroutine and get the result back, but the problem with async arises when we do not want to make parallel network calls.
* It is known when async is used, one needs to use the await() function, which leads to blocking of the main thread, 
* but here comes the concept of withContext which removes the problem of blocking the main thread.
* withContext is nothing but another way of writing the async where one does not have to write await(). 
* When withContext, is used, it runs the tasks in series instead of parallel. So one should remember that when we have a single task in the background and want to get back the result of that task, we should use withContext

__eg:__
__// two kotlin suspend functions
// Suppose we have two tasks like below__

```private suspend fun doTaskOne(): String
{
delay(2000)
return "One"
}```

private suspend fun doTaskTwo(): String
{
delay(2000)
return "Two"
}

 

__// kotlin function using async__
fun startLongRunningTaskInParallel() 
{
  viewModelScope.launch 
  {
    val resultOneDeferred = async { TaskOne() }
    val resultTwoDeferred = async { TaskTwo() }
    val combinedResult = resultOneDeferred.await() + resultTwoDeferred.await()
  }
}

__// kotlin function using withContext__
fun startLongRunningTaskInParallel()
{
viewModelScope.launch
{
	val resultOne = withContext(Dispatchers.IO) { TaskOne() }
	val resultTwo = withContext(Dispatchers.IO) { TaskTwo() }
	val combinedResult = resultOne + resultTwo
}
}

   
## withContext(Dispatchers.Default) 
which context also used to switch threads means when we perform task on background or on some other thread and wants to udpdate UI on main thread
__eg:__
private var _data=MutableLiveData<Int>()
_data.value= withContext(Dispatchers.IO)
{
getData()
 Log.i(
 TAG,
 "runCoroutine: " + Thread.currentThread().name
)// DefaultDispatcher-worker-1
 }
 
 __Note:__ By default coroutine runs on main thread if you want to perform task on diff thread we can use withContext with below Dispatchers 
* Dispatchers.IO //works on DefaultDispatcher-worker-1 Thread 
* Dispatchers.Main //works on Main Thread 
* Dispatchers.Default //works on DefaultDispatcher-worker-1 Thread 
* Dispatchers.Unconfined //works on kotlinx.coroutines.DefaultExecutor Thread 

## Exception Handling in coroutines
* try-catch
* CoroutineExceptionHandler





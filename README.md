# CoroutinesBasic
## Coroutine Scopes:
* GlobalScope: Global scope is used to launch top-level coroutines which are operating on the whole application lifetime and are not cancelled prematurely
* runBlocking :  It will get run on the main thread,Runs a new coroutine and blocks the current thread until its completion.
* viewModelScope : A ViewModelScope is defined for each ViewModel in your app. Any coroutine launched in this scope is automatically canceled if the ViewModel is cleared.
* coroutineScope: CoroutineScope is a starting Point of Coroutine. CoroutineScope can have more than one coroutine within itself, which makes coroutine hierarchy. 
* supervisiorScope: A supervisorScope won't cancel other children when one of them fails while CoroutineScope does.

## Coroutine Bulders:
* launch : Can we use to execute any operation synchronously
* Async  : Can we use to execute any operation aSynchronously
Points to remembr:
o It is known that async and launch are the two ways to start the coroutine. 
o Since It is known that async is used to get the result back, & should be used only when we need the parallel execution,
o whereas the launch is used when we do not want to get the result back and is used for the operation such as updating of data, etc.
o As we know that async is the only way till now to start the coroutine and get the result back, but the problem with async arises when we do not want to make parallel network calls.
o It is known when async is used, one needs to use the await() function, which leads to blocking of the main thread, 
o but here comes the concept of withContext which removes the problem of blocking the main thread.
o withContext is nothing but another way of writing the async where one does not have to write await(). 
o When withContext, is used, it runs the tasks in series instead of parallel. So one should remember that when we have a single task in the background and want to get back the result of that task, we should use withContext

eg:
// two kotlin suspend functions
// Suppose we have two tasks like below

private suspend fun doTaskOne(): String
{
delay(2000)
return "One"
}

private suspend fun doTaskTwo(): String
{
delay(2000)
return "Two"
}

 

// kotlin function using async
fun startLongRunningTaskInParallel() 
{
  viewModelScope.launch 
  {
    val resultOneDeferred = async { TaskOne() }
    val resultTwoDeferred = async { TaskTwo() }
    val combinedResult = resultOneDeferred.await() + resultTwoDeferred.await()
  }
}

// kotlin function using withContext
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
eg:
private var _data=MutableLiveData<Int>()
_data.value= withContext(Dispatchers.IO)
{
getData()
 Log.i(
 TAG,
 "runCoroutine: " + Thread.currentThread().name
)// DefaultDispatcher-worker-1
 }
 
 Note: By default coroutine runs on main thread if you want to perform task on diff thread we can use withContext with below Dispatchers 
o Dispatchers.IO //works on DefaultDispatcher-worker-1 Thread 
o Dispatchers.Main //works on Main Thread 
o Dispatchers.Default //works on DefaultDispatcher-worker-1 Thread 
o Dispatchers.Unconfined //works on kotlinx.coroutines.DefaultExecutor Thread 

## Exception Handling in coroutines
o try-catch
o CoroutineExceptionHandler





# Progress so far, Sources of error and near-term goals for perfection of task 2.

So , the point of this program is to simulate noise artificial noise using X, Y and Z gates only. The transpilation gate basis is given. Now using our `add_sum` function we have to perform binary addition. the noise addition and basis gate transpilation is working perfectly. The only focus right now is to write the `add_sum` function correctly and verify the same from the draper addition paper. I haven't yet seen the general quantum arithmetic paper because it had additional circuits for multiplication as well as exponentiation. All we need to do for this task is addition. Draper addition uses QFT via the steps :  \
$a \rightarrow QFT(a) \\ b \rightarrow QFT(a+b) \rightarrow IQFT(QFT(a+b)) = a+b \rightarrow$ appropriate statevector. \
\
After doing so we also have to run this circuit in a sampler and return the results. There should be two results: one of them ideal i.e the **transpilation circuit** function for example 1+2 = 3 :: 01 + 10 == 11 and the another non-ideal which should have the noisy results. This should be shown by the transpilation circuit and we have to **plot** the results as well. The noisy circuit because of noise should show all kinds of counts. The ingenuity of our task is exactly there that the "induced" randomness is completely `np.random`'s call in the `noisyy` function which is randomly adding noise to the final addition result. 

For generality my plans are to keep the transpiled function intact and instead of our simulated noise we also try our circuit against depol noise and test it in real backend. And finally plot beautifully all our results one which shows accurate results for the quantum arithmetic, one which shows "simulated noise", one which shows depol noise, and finally one that shows noise from Real IBM Backend.

Another mission is to make this program capable of addition upto at least 5 bit numbers. That should be more than enough.

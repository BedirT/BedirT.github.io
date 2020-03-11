---
layout: post
title: "Factorizing a Number"
date: 2016-11-20 17:00
excerpt_separator: "<!--more-->"
categories: [Algorithms]
tags: [Competitive programming]
comments: true
---

**Before starting-** What is Prime Factorization ? What is a Prime number ? If you are curious about these please checkout this link before proceed because I will not explain them here :)

<!--more-->

[Wikipedia - Prime Numbers](https://en.wikipedia.org/wiki/Prime_number)

<img src="/img/blogImages/Efficient_Prime.jpg">

Since we all know what a prime number and composite number is, let's look at our realllly simple algorithm. Actually there is nothing fancy here, we are just using simple Sieve of Eratoshenes(Hardest name to pronounce, I checked online if I am right) algorithm. By the way that topic also pre-requised for this post, but fortunetely we already have a tutorial-explanation for it. If you don't know or confused about it in some ways please check the links below:

[Our Series Blog Post about Sieve of Erothene...](https://github.com/NAU-ACM/ACM-ICPC-Preparation/blob/master/Week1/Sieve-of-Eretosthenes/README.md)

[Also Check Here](https://github.com/NAU-ACM/ACM-ICPC-Preparation/tree/master/Week1)

Since "WE" covered everything required, let me involve in this learning process too... Prime factorization: This is highly important topic. All your passwords , your bank accounts and stuff are protected by these numbers. Anyway that is why we actually have couple algorithms about Prime Factorization. There is a good answer on [quora.com](quora.com) about the prime number algorithms:

> Different algorithms get used based on how large the number is. It goes something like this:
>
> Small Numbers : Use simple sieve algorithms to create list of primes and do plain factorization. Works blazingly fast for small numbers.
>
> Big Numbers : Use Pollard's rho algorithm, Shanks' square forms factorization (Thanks to Dana Jacobsen for the pointer)
>
> Less Than  10^25  : Use Lenstra elliptic curve factorization
>
> Less Than  10^100  : Use Quadratic sieve
>
> More Than  10^100  : Use General number field sieve
>
> Currently, in the very large integer factorization arena, GNFS is the leader. It was the winner of the RSA factoring challenge of the 232 digit number
>
> *Arun Iyer* [quora.com](https://www.quora.com/Which-is-the-fastest-prime-factorization-algorithm-to-date)

OK cool, we have a lot of options, although you can see that these numbers are gigantic. $$10^{25}$$ ?? This was the smallest one mentioned above by the way. So we don't really care about them, they are exist because like I said before, these numbers are extremely powerful so people need biiig ones. Since our languages supports (for C++) until $$10^{19}$$ , and our tutorials are for ACM-ICPC kind programming contests, considering that these contests have time limit and %100 sure that if $$10^{25}$$ will given... you probably should search for some trick in question, because we cannot compete that many operations on time.

## Finally the Algorithm

Anyway after all explanation lets talk about our "small" algorithm. It really is nothing much than using Sieve algorithm. We are just going to optimize it a little bit. Let's say we already runned our sieve function:

```cpp
sieve(10001);
```

Now we have an array or vector , I don't know how you implemented so I will go with mine -> you can check it out:

```cpp
vector<int> primes;

void sieve(int size) {
	bitset<10000010> was; 	// You can also use boolean array or vector, but this is optimized for bool (C++ is best :) )
	was.set();        		// Initilizing all bitset to true
	was[0] = was[1] = 0;	// Except 0 and 1 of course 
	for (int i = 2; i <= size; i++) 
  		if (was[i]) {
  			primes.push_back(i);
	    		for (int j = i * i; j <= size; j += i) was[j] = 0;
  		}
}
```

We have a vector named primes and it has all the primes from begining(2) to size.

$$primes -> [ 2 , 3 , 5 , 7 , 11 , 13 , 19 , 21 , ... ]$$

What will we do is we will use basic logic and check every prime number and if it can divide our number **N**. If it can divide , we will just put it into our new vector (If you don't know vector you still can use list or array, depends on the language). If we can divide we will divide it, with this way we will decrement our operations. So let's say we have **18** as our **N**. We start with first element in the **_primes_** which is **2**. 

- Is **2** dividing **N = 18** ?

Yes obviously so:

- Put **2** into our **Factors** vector;

So -> 

$$Factors -> [ 2 ]$$

And we will divide our N by 2:

- **N = 18/2 = 9**

Continue to check if 2 is dividing N which is not becuese **N = 9**. So lets pass 2 and go to 3:

- Is **3** dividing **N = 9** ? Yes
- Put **3** into our **Factors** vector;

$$Factors -> [ 2 , 3 ]$$

- **N = 9/3 = 3**
- Is **3** dividing **N = 3** ? Yes
- Put **3** into our **Factors** vector;

$$Factors -> [ 2 , 3 , 3 ]$$

- **N = 3/3 = 1**
- Is **3** dividing **N = 1** ? Nope
- Proceed to **5**.

**5** ? Yes we will stop here. This is the next optimization, at most we will go until $$p^2 \leq N$$ (and p is my prime number that I am checking). This is what determines my complexity in this method. So I have $$\sqrt{N}$$ here. This is also my number that will go into O notation -> O($$\sqrt{N}$$). (Mathematically this complexity is represented with $$O(\pi(\sqrt{N})) = O(\sqrt{N}\times lnN)$$)You can further check the code C++ implementation. I commented it so you can see what is going on in each step. After understanding the code I highly recommend you to solve questions about this topic, we have our list for this question as well, check the link at the bottom.

## Implementation C++

```cpp
vector<int> primeFactors(int N){
	vector<int> vc; // An empty vector for us to fill with our numbers factors.
	int idx = 0, f = primes[idx]; // f standing for FACTOR - idx is index that we will increment 
	while(N != 1 && N >= f * f){  // f * f ... This is the part with sqrt(N) so the loop continues until our factor is bigger than sqrt(N)
		while(N % f == 0){ 	// I will continuously check if N is divisible by this prime, until it become wrong.
			N /= f; 			// Dividing N to my prime.
			vc.push_back(f); 	// adding that prime to my vector.
		}
	}
	if(N != 1) vc.push_back(N); 	// This case is for prime numbers itself, if the number is prime than we should add it to our vector. 
							// If some value, after our loop is still not equals to 1 than it is a prime itself. (because of sqrt(N))
	return vc;
}
```
We have a well designed Curriculum on Github, also the questions about this algorithm are there too, check it out here

[ACM-ICPC Curriculum](https://github.com/NAU-ACM/ACM-ICPC-Preparation) 

[Week 1 - Which includes this topic](https://github.com/NAU-ACM/ACM-ICPC-Preparation/tree/master/Week1)



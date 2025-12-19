This is a githib repository (technically a fork)
of Rrdrand, taken from
https://cran.r-project.org/src/contrib/Archive/Rrdrand/

It is archived and not available on live CRAN because of test errors which are noted here:

https://cran-archive.r-project.org/web/checks/2023/2023-01-26_check_results_Rrdrand.html

These do not affect x86_64 architecture using gcc, so on that platform it will install fine.

However the warning complaint about not having a prototype, well I tried to reproduce it with clang
but it didn't complain either, though I did not have pedantic warnings on.

In the event, it's little problem to stick in a prototype, which I have done.

So what does this package do? Well, nothing if the CPU does not have the RDRAND hardware device.

There's a base R function called RNGkind() which will tell you waht's the basis of random number generation
and typically it says

> RNGkind()
[1] "Mersenne-Twister" "Inversion"        "Rejection"

Actually it's quite well known that R uses the Mersenne twister, which was pretty great in the 2000s but
there have been advances since then and it's viewed as a little slow and outdated now with several new alternatives
not least of which is an actuall hardware RNG in intel's x86_64 CPU called RDRAND.

Now if you do have RDRAND, well you will type and get the following
> RNGkind()
[1] "user-supplied" "Inversion"     "Rejection"

SO the labelling here is not great, but the user-supplied would seem to refer to Rrdrand

and the Rrdrand's doc then ask you to
> print(runif(3))
[1] 0.001030267 0.892176375 0.716576358

So the understanding is that those are RDRAND generated.

There is something going on under hte hood here though
user_unif_rand()
is the operational function (the only one - it's a light package)
so it prehaps only refers to runif()?
And even so is it overloading a base R function.
which obviously R is prepared for? a plugin so to speak.

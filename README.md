# Alchemy2 docker image
Docker image for Alchemy2 MLN software (`learnstruct`, `learnwts` and `infer`)

http://alchemy.cs.washington.edu/

Note: Follow this [doc](COMPILE_ALCHEMY.md) if you prefer to compile Alchemy on Ubuntu7, instead of using a Docker image.


Put your MLN and db files in a directory such as `/Users/david/data`. For instance:

```
$ cd /Users/david
$ mkdir data
$ cd data
$ wget https://alchemy.cs.washington.edu/mlns/tutorial/soc-net/smoking.mln
$ wget https://alchemy.cs.washington.edu/data/tutorial/smoking/smoking-train.db
$ wget https://alchemy.cs.washington.edu/data/tutorial/smoking/smoking-test.db
```

Then download and start a docker container with the alchemy software installed:

```
$ docker run -ti --name alchemy2 -v /Users/david/data/:/data dportabella/docker-alchemy2 bash
```

Now you can use alchemy in the container:

```
root@287784dd2500:/# mkdir /data/result/


root@287784dd2500:/# learnwts -d -i /data/smoking.mln -o /data/result/smoking-out.mln -t /data/smoking-train.db -ne Smokes,Cancer
...
Done learning discriminative weights.
Time Taken for learning = 6.14 secs
Total time = 6.14 secs


root@287784dd2500:/# infer -ms -i /data/result/smoking-out.mln -r /data/result/smoking.result -e /data/smoking-test.db -q Smokes,Cancer
...
Done MC-SAT sampling. 1000 samples.
Final ground predicate number: 10
Final ground clause number: 16
total time taken = 0.04 secs


# exit the container
root@287784dd2500:/# exit
```

Back in the host computer, you can delete the docker container and image:

```
$ docker rm alchemy2
$ docker rmi dportabella/docker-alchemy2
```

You have the results in your host computer:

```
$ ls /Users/david/data/result/
smoking-out.mln	smoking.result


$ cat /Users/david/data/result/smoking-out.mln
//predicate declarations
Cancer(person)
Friends(person,person)
Smokes(person)

// 0.892755  Smokes(x) => Cancer(x)
0.892755  !Smokes(a1) v Cancer(a1)

// 0.883091  Friends(x,y) => (Smokes(x) <=> Smokes(y))
0.441545  !Friends(a1,a2) v Smokes(a1) v !Smokes(a2)
0.441545  !Friends(a1,a2) v Smokes(a2) v !Smokes(a1)

// 0       Friends(a1,a2)
0       Friends(a1,a2)

// 0.537004  Smokes(a1)
0.537004  Smokes(a1)

// -1.33406  Cancer(a1)
-1.33406  Cancer(a1)


$ cat /Users/david/data/result/smoking.result
Smokes(John) 0.728977
Smokes(Katherine) 0.433007
Smokes(Lars) 0.464004
Smokes(Michael) 0.89796
Cancer(Ivan) 0.409009
Cancer(John) 0.375012
Cancer(Katherine) 0.230027
Cancer(Lars) 0.306019
Cancer(Michael) 0.429007
Cancer(Nick) 0.409009
```

About Alchemy:

- http://alchemy.cs.washington.edu/
- http://alchemy.cs.washington.edu/tutorial/tutorial.pdf
- https://alchemy.cs.washington.edu/mlns/tutorial/

Disclaimer: I am not affiliated to the authors of the [Alchemy software](http://alchemy.cs.washington.edu/).

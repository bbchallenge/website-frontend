<SeoTitle value="Story" />

<script lang="ts">
import { onMount } from 'svelte';
import SeoTitle from "$lib/seo_title.svelte";
import TmSimulator from "$lib/tm_simulator.svelte"
import { machineCodeToTM, tmToTuringMachineDotIO  } from '$lib/tm';
import {BB5_champion} from '$lib/machine_repertoire'
import Katex from "$lib/Katex.svelte"

let theCode = tmToTuringMachineDotIO(machineCodeToTM(BB5_champion))

onMount(() => { // TODO: this shouldn't be necessary
    const id = window.location.hash.replace(/^#/, '');
    const element = id && document.getElementById(id);
    console.log(id,element)
    if (id && element) {
      window.scrollTo({ top: element.top, behavior: 'smooth' });
    }
  });

// Cannot inline { .. } because of svelte
let nbTM = "(4n+1)^{2n}"
let nbTM5 = "21^{10} \\simeq 1.6\\times 10^{13}"
let BB7 = "\\geq 10^{10^{10^{18,705,352}}}"
let BB72 = "10^{10^{10^{18,705,352}}}"
let BB6 = "> 10 \\uparrow \\uparrow {15}"

</script>

<div class="dark w-full ">
<div class="prose prose-invert text-white -mt-4  xl:justify-start lg:ml-[170px] ml-0 sm:ml-4 font-sans prose-base sm:prose-lg w-full">
<div class="leading-normal ">
<div>

## Table of contents


## Goal

<a id="goal"></a>

<div class="flex justify-center m-0 -mt-10">
<img
					src="/branding/bbchallenge_logo_no_margins.svg"
					alt="The Busy Beaver Challenge's logo"
					class="h-[190px] m-0 p-0"
				/>
</div>

<div class="bg-blue-500 p-2"><strong>Disclaimer:</strong> This page is specific to the search of BB(5), which has been completed as of July 2nd 2024.</div>

The goal of the Busy Beaver Challenge (bbchallenge for short) is to collaboratively prove or disprove the following conjecture [[Marxen and Buntrock, 1990]](http://turbotm.de/~heiner/BB/mabu90.html) [[Aaronson, 2020]](https://www.scottaaronson.com/papers/bb.pdf):

<div class="flex justify-center">
BB(5) = 47,176,870
</div>

This conjecture says that if a 5-state 2-symbol [Turing machine](#turing-machines) runs for more than 47,176,870 steps without halting then it will never halt (starting from all-0 memory tape).

The conjecture, explicitly formulated in [[Aaronson, 2020]](https://www.scottaaronson.com/papers/bb.pdf), is based on the pioneering work of [[Marxen and Buntrock, 1990]](http://turbotm.de/~heiner/BB/mabu90.html) who discovered the current <a  href="https://bbchallenge.org/{BB5_champion}&w=250&ox=0.8&status=halt" rel="external">5-state busy beaver champion</a>, a machine with 5 states that halts after 47,176,870 steps:

<div class="flex flex-col items-center">
<div class="w-1/3 -mt-5 font-mono">

|     | 0   | 1   |
| --- | --- | --- |
| A   | 1RB | 1LC |
| B   | 1RC | 1RB |
| C   | 1RD | 0LE |
| D   | 1LA | 1LD |
| E   | --- | 0LA |

</div>
</div>

Achieving the goal of the Busy Beaver Challenge implies studying **88,664,064 Turing machines** and decide whether they halt or not, see <a href="/method" rel="external">Method</a>.

<a href="/contribute" rel="external">You can help!</a>

<a id="turing-machines"></a>

## Turing machines

The introduction of Turing machines by Alan Turing in 1936 is arguably one of the founding events of computer science, [[Turing, 1936]](https://www.cs.virginia.edu/~robins/Turing_Paper_1936.pdf).

Turing machines can be thought as a primitive computer architecture providing (i) a runtime environment consisting of a moving read/write head over a memory tape divided in adjacent memory cells and (ii) a programming language allowing to control the behavior of the head depending on the content of the memory cell where it is currently at.

The Turing machines we work with have one bi-infinite memory tape where each cell is either containing a 0 or a 1.

The programmer specifies the code of the machine in a table with 2 columns and <span class="math math-inline">q</span> rows. Rows are called **states**. Here is the code of the current <a  href="https://bbchallenge.org/{BB5_champion}&w=250&ox=0.8&status=halt">5-state busy beaver champion</a>:

<div class="flex flex-col items-center -mb-2">
<div class="w-1/3 -mt-5 font-mono">

|     | 0                                                             | 1   |
| --- | ------------------------------------------------------------- | --- |
| A   | <span class="bg-purple-400 font-bold text-black">1RB</span>   | 1LC |
| B   | 1RC                                                           | 1RB |
| C   | 1RD                                                           | 0LE |
| D   | 1LA                                                           | 1LD |
| E   | <span class="bg-green-400 text-gray-900 font-bold">---</span> | 0LA |

</div>
</div>

Each entry of the table is called a **transition** and instructs us what to do when reading a 0 or a 1 at the current head's position on the memory tape in a given state. For instance, in the above machine, <span class="bg-purple-400 font-bold text-black">&nbsp;reading a 0 in state A&nbsp;</span> will:

1. write a 1 at the current head position (i.e replacing the read 0 with a 1)
2. move the head to the right
3. jump to state B for the next instruction

The machine will **halt** (i.e. cease functioning) if it ever tries to execute an **undefined transition** denoted by **---**. Here, it would happen only if ever <span class="bg-green-400 text-gray-900 font-bold">&nbsp;reading a 0 in state E&nbsp;</span>.

In the context of the Busy Beaver Challenge, machines are always executed starting in state A and with a memory tape that is initially all 0 (i.e. all memory cells are 0).

<a id="interactive-simulator"></a>

### Interactive simulator

As with probably any programming language, the best way to understand Turing machines is to play with them:

<TmSimulator/>

A more detailed simulator is available at <a href="https://turingmachine.io" target="_all 0">https://turingmachine.io</a>. Here is the code of the above machine in their format:

<pre>{ theCode }</pre>

<a id="space-time-diagrams"></a>

### Space-time diagrams

Space-time diagrams provide a condensed way to visualise the behavior of [Turing machines](#turing-machines). The space-time diagram of a machine is a 2D image where the i<sup>th</sup> row represents the memory tape of the machine at the i<sup>th</sup> iteration. Black pixels are used for memory cells containing 0 and white for 1.

Here is the space-time diagram of the first 10,000 iterations of the <a  href="https://bbchallenge.org/{BB5_champion}&w=250&ox=0.8&status=halt" rel="external">5-state busy beaver champion</a>:

<div class="flex justify-center -mt-16 -mb-8">

![](./bb5.png)

</div>

Additional colors are used to track the head position and its movement: green when the head has moved to the left and red when it has moved to the right.

By default, these space-time diagrams are re-scaled to fit a 400x500 canvas, hence they can be inexact due to the scaling algorithm, especially at small scales (i.e. few simulation steps).

You can tick **Simulation Parameters** to modify this head coloration and the size of the simulated space/time axes.

You can tick **Explore mode** to enter a more precise visualisation of space-time diagrams that are accurate at small scales and that will give you additional coloured information about the state of the machine at each step.

<a id="machine-id"></a>

#### Machine ID

The Busy Beaver Challenge is based on a <a href="/method#seed-database" rel="external">seed database</a> of 88,664,064 undecided 5-state machines, see <a href="/method" rel="external">Method</a>. We can consequently refer to undecided machines with their ID in this database.

For instance: <a  href="https://bbchallenge.org/55897188" rel="external">https://bbchallenge.org/55897188</a>

<!-- #### Runtime

Turing machines have to physically move their head to a memory cell before they can read or write the data located there. This contrasts with the Random Access Memory (RAM) architecture used by modern computers where any _random_ memory cell can be accessed instantly given its address. Nonetheless, any algorithm that can be implemented using a modern RAM computer can be implemented with a Turing machine (and vice versa). -->

<a id="will-it-halt-or-not"></a>

### Will it halt or not?

Turing machines have an important property: starting from a given memory tape (all-0 in our case), they either **halt** or don't. By halting we mean that the machine tries to execute an undefined transition and, since it is undefined, stops functioning. Here is a machine that halts after 4 steps:

<TmSimulator machineCode="1RB1RB_1LA---_------_------_------"/>

Here is another machine that halts after 105 steps:

<TmSimulator machineCode="1RB1LC_0LB1LA_1RD1LB_1RE0RD_0RA---"/>

If a machine has no undefined transition it is sure that it will never halt as it cannot ever encounter an undefined transition.

However, it is not because a machine has an undefined transition that it will halt one day. The most simple example to support this statement is the following machine that will never halt starting from all-0 tape although it has plenty undefined transitions:
<!-- ^[1. This machine can be thought as the "while true" of Turing machines.]  -->


<TmSimulator machineCode="0RA---_------_------_------_------"/>

In this case it is easy to convince ourselves that the machine will never halt starting from all-0 tape. However, if we take our earlier example:

<TmSimulator/>

Will this one halt or not starting from all-0 tape?

Answering this question does not look simple. Here, patience can answer it for us because the machine **does halt**, after <span class="bg-[#1162D3] px-0.5">47,176,870</span> steps. However, this fact would have been quite difficult to predict just from looking at the code of the machine.

To this day, no 5-state Turing machine is known to halt after more than <span class="bg-[#1162D3] px-0.5">47,176,870</span> steps.

With the Busy Beaver Challenge, we hope to discover if that <span class="bg-[#1162D3] px-0.5">47,176,870</span> record can be beaten or not among 5-state machines. This implies to decide, for all machines with 5 states, whether they halt or not.

But why focusing on 5 states? Let's first reformulate the problem in terms of busy beavers.

<a id="the-busy-beaver-function-bb"></a>

## The busy beaver function BB

<a id="definition-of-bb"></a>

### Definition of BB

We can now properly define the busy beaver function BB (called S originally) as introduced in [[Rado, 1962]](https://cs.famaf.unc.edu.ar/~hoffmann/cc18/Rado-On-non-computable.pdf):

<div class="flex justify-center items-center space-x-2">
<div>
BB(n) = 
</div><div class="text-sm w-[300px]">Maximum number of steps done by a halting Turing machine with n states starting from all-0 memory tape</div>
</div>

Note that there is a finite number of Turing machines with n states, <Katex math={nbTM}/> to be exact, hence BB(n) is well defined.

BB(n) is a very powerful number to know because it gives you a way to decide any machine with n states: run the machine and if it runs for BB(n)+1 steps you know that it will never halt.

This means that the function n ↦ BB(n) is not computable otherwise [the halting problem of Turing machines](https://en.wikipedia.org/wiki/Halting_problem) would be decidable. Find more properties of BB in [[Aaronson, 2020]](https://www.scottaaronson.com/papers/bb.pdf).

<a id="what-is-known-about-bb"></a>

### What is known about BB

Only 4 values of BB are known:

- BB(1) = 1, [[Rado, 1962]](https://cs.famaf.unc.edu.ar/~hoffmann/cc18/Rado-On-non-computable.pdf)
- BB(2) = 6, [[Rado, 1962]](https://cs.famaf.unc.edu.ar/~hoffmann/cc18/Rado-On-non-computable.pdf)
- BB(3) = 21, [[Rado and Lin, 1963]](https://etd.ohiolink.edu/apexprod/rws_etd/send_file/send?accession=osu1486554418657614&disposition=inline)
- BB(4) = 107, [[Brady, 1983]](https://www.jstor.org/stable/2007539)

The fact that BB(4) = 107 means that if a 4-state Turing machine does not halt after 107 steps starting from all-0 tape then it will never halt.

Proving the value of BB(n) implies to be able to decipher the behavior of any machine with n-states (starting from all-0 tape). The number of machines grows exponentially with n hence making the task overwhelmingly hard very quickly.

Currently, BB(5) is unknown but is conjectured to be BB(5) = 47,176,870 [[Marxen and Buntrock, 1990]](http://turbotm.de/~heiner/BB/mabu90.html) [[Aaronson, 2020]](https://www.scottaaronson.com/papers/bb.pdf). The naïve space of 5-state Turing machines contains <Katex math={nbTM5}/> machines, see <a href="/method#machine-format" rel="external">Method</a> for how we can reduce and search this space efficiently.

Apart from concrete values of BB, the following is also known:

- BB(5) >= 47,176,870 [[Marxen and Buntrock, 1990]](http://turbotm.de/~heiner/BB/mabu90.html)
- BB(6) <Katex math={BB6}/> [[Kropitz, 2022]](https://www.sligocki.com/2022/06/21/bb-6-2-t15.html)
- BB(15) is at least as hard as Erdős' conjecture on powers of 2: "for n > 8, there is at least one digit 2 in the base-3 representation of 2<sup>n</sup>". [[Stérin and Woods, 2021]](https://arxiv.org/pdf/2107.12475.pdf)
- BB(27) is at least as hard as Goldbach conjecture: "for n > 2, every even integer is the sum of two primes" [unverified construction](https://gist.github.com/anonymous/a64213f391339236c2fe31f8749a0df6) [[Aaronson, 2020]](https://www.scottaaronson.com/papers/bb.pdf)
- BB(744) is at least as hard as Riemann Hypothesis [[Matiyasevich and O'Rear and Aaronson, unpublished]](https://github.com/sorear/metamath-turing-machines/blob/master/riemann-matiyasevich-aaronson.nql)
- BB(748) is independent of ZF [[O'Rear, unpublished]](https://github.com/sorear/metamath-turing-machines/blob/master/zf2.nql)
- BB(5,372) is at least as hard as Riemann Hypothesis [[Yedidia and Aaronson, 2016]](https://arxiv.org/abs/1605.04343)
- BB(7,910) is independent of ZFC [[Yedidia and Aaronson, 2016]](https://arxiv.org/abs/1605.04343)

All these results come from constructing explicit Turing machines. For instance, in [[Stérin and Woods, 2021]](https://arxiv.org/pdf/2107.12475.pdf), the authors construct an explicit 15-state Turing machine which enumerates all powers of two in base 3 until it finds a counterexample to Erdős' conjecture on powers of 2. Hence the machine halts if and only if the conjecture is false!

Knowing the value of BB(15) would imply that we'd know if that particular 15-state Turing machine halts or not, it means that knowing BB(15) is at least as hard as solving Erdős' conjecture.

<a id="the-busy-beaver-scale"></a>

#### The busy beaver scale

These results provide a scale, **the busy beaver scale**, on which we can measure the complexity of various mathematical problems. For instance, according to this scale (and current knowledge), Erdős' conjecture on powers of 2 is less complex than Goldbach conjecture since it can be encoded as the halting problem of a smaller Turing machine.

These results also drastically reduce the hope that we'd ever know the value of BB even for small values such as 15. Even worse, BB(6) <Katex math={BB6}/> [[Kropitz, 2022]](https://www.sligocki.com/2022/06/21/bb-6-2-t15.html) as there is a 6-state Turing machine halting after roughly that many steps, which is way bigger than the estimated number of atoms in the universe 10<sup>80</sup>.

Hence, the frontier between tractable and intractable values of BB seems to be situated at BB(5).

<a id="bb5"></a>

### BB(5)

The above motivates the Busy Beaver Challenge: **let's try to collaboratively find BB(5)**, the smallest currently unknown BB value.

Prior work exhibited the current <a  href="https://bbchallenge.org/{BB5_champion}&w=250&ox=0.8&status=halt" rel="external">5-state busy beaver champion</a> halting after 47,176,870 steps [[Marxen and Buntrock, 1990]](http://turbotm.de/~heiner/BB/mabu90.html) which has not been beaten in the past 30 years.

This led to [[Aaronson, 2020]](https://www.scottaaronson.com/papers/bb.pdf) conjecturing that BB(5) = 47,176,870.

Go to <a href="/method" rel="external">Method</a> and <a href="/contribute" rel="external">Contribute</a> to see how we plan to find BB(5) and how you can contribute.

Are you up for the challenge?

<a id="possible-outcomes-of-the-challenge"></a>

## Possible outcomes of the challenge

Here are some possible outcomes to the quest of looking for BB(5):

- We decide the halting problem of all 5-state machines (from all-0 tape), see <a href="/method#machine-format" rel="external">Method</a>, which as a result gives the value of BB(5) 🥳

- We find a 5-state machine that halts after more than 47,176,870 steps hence improving Aaronson's conjecture [[Aaronson, 2020]](https://www.scottaaronson.com/papers/bb.pdf) 🥳

- We establish a fine-grained [Zoology](/#zoology) of the behaviors of 5-state Turing machines which allows us to understand what they are capable of and where complexity lies 🥳

- We decide as many machines as we can but fail to decide some of them. Their individual halting problems compete for the title of "simplest open problem in mathematics" (on the [busy beaver scale](#the-busy-beaver-scale)) which is also 🥳

<a id="similar-projects"></a>

## Similar projects

<a id="skelets-43-undecided-machines"></a>

### Skelet's 43 undecided machines

In 2003, [Skelet](https://skelet.ludost.net/bb/nreg.html) released ≈6000 lines of Pascal that aim at achieving the same result as the Busy Beaver Challenge (but ~20 years before): decide the behavior of all 5-state Turing machines from all-0 tape. Skelet's program left only 164 machines undecided which he reduced further to only 43 "hardly non-regular" machines. This is claimed to have since been reduced even further to a mere 21 machines by various authors including [Dan Briggs](https://github.com/danbriggs/Turing/blob/master/paper/HNRs.pdf). Significant work has been made by these authors to manually decide the behavior of Skelet’s 43 machines.

This represents a substantial and truly impressive effort.

However, we do not believe that Skelet’s program has been reviewed independently, and proving 6,000 lines of uncommented Pascal correct would be difficult (How can we be sure that the original set of 164 machines is not erroneous? I.e we'd need a proof that some machines were not overlooked or nor decided using incorrect arguments.).

In contrast, it is one of the core mission of the Busy Beaver Challenge to provide collaborative, open source, concise, modular, tested and proved correct code in order to facilitate peer-review and increase trust in the final outcome of the challenge. See our <a href="/method#reproducibility-and-verifiability-statement" rel="external">reproducibility and verifiability statement</a>.

The full list of Skelet's 43 machines and their equivalents in the Busy Beaver Challenge <a href="/skelet" rel="external">is available here</a>.

<a id="hertels-100-holdouts"></a>

### Hertel's 100 holdouts

In 2009, Joachim Hertel [published a method](https://content.wolfram.com/uploads/sites/19/2009/11/Hertel.pdf) that claims to leave only 100 machines undecided. The method seems to be very robust and an independent reproduction of these results would be very interesting.

<a id="references"></a>

## References

- [[Aaronson, 2020]](https://www.scottaaronson.com/papers/bb.pdf)
- [[Brady, 1983]](https://www.jstor.org/stable/2007539)
- [[Cloudy176, 2014]](https://googology.fandom.com/wiki/User_blog:Cloudy176/Proving_the_bound_for_S(7))
- [[Marxen and Buntrock, 1990]](http://turbotm.de/~heiner/BB/mabu90.html)
- [[Michel, 2009]](https://arxiv.org/abs/0906.3749#:~:text=Pascal%20Michel%20(ELM),faster%20than%20any%20computable%20function.)
- [[Rado, 1962]](https://cs.famaf.unc.edu.ar/~hoffmann/cc18/Rado-On-non-computable.pdf)
- [[Rado and Lin, 1963]](https://etd.ohiolink.edu/apexprod/rws_etd/send_file/send?accession=osu1486554418657614&disposition=inline)
- [[Stérin and Woods, 2021]](https://arxiv.org/pdf/2107.12475.pdf)
- [[Yedidia and Aaronson, 2016]](https://arxiv.org/abs/1605.04343)
- [[Wythagoras, 2014]](https://googology.fandom.com/wiki/User_blog:Wythagoras/A_good_bound_for_S(7)%3F)

<a id="related-links"></a>

## Related Links

- [Heiner Marxen's website](http://turbotm.de/~heiner/BB/index.html)
- [Pascal Michel's website](https://bbchallenge.org/~pascal.michel/index.html)
- [Rensselaer RAIR Lab](https://homepages.hass.rpi.edu/heuveb/Research/BB/index.html)
- [Skelet's website](https://skelet.ludost.net/bb/index.html)
- [Busy Beaver Discuss](https://groups.google.com/g/busy-beaver-discuss) (Google Groups)


<div class="mb-20"></div>

</div>
</div>
</div>
</div>

# Omni workshop and a review

### [James Bradbury](https://www.jamesbradbury.net)
---

A couple of weeks ago the creative coding hackspace was conducted online via [Jitsi](meet.jit.si). We had a great turnout with people attending from all over the UK and even a parking lot in Florida, US (Thanks Jorge). In this online hacking session we gave control over to [Francesco](github.com/vitreo12) who gave us a high octane crash course in his new domain specific language called [`omni`](https://vitreo12.github.io). I have to say, `omni` is really elegant and I'm excited to use it for most of my DSP needs in the future. The code that you write in `omni` is compiled to shared or static libraries that can then be accessed or compiled elsewhere, making one of the key pros of this DSL its portability. So far Francesco has created wrappers for both Max and SuperCollider where `omni` code is compiled to objects and ugens respectively. The wrapper also takes away a lot of the complexity of developing for these environments allowing you focus on the DSP side of things.

After some initial set up time, compiling and installation a bunch of were hacking away and creating delays, synthesisers, filters and generally  having fun coding. If you follow [this link](github.com/jamesb93/omnicode) my amateur DSP skills and imagination will become redily available for you to see. Inside though, you'll find a number of `.omni` files that captured some of the things that I wrote in the hackspace. I think the simplicity of `omni` speaks for itself when this is all it takes to write a nice cubic non-linear distortion for both Max and SuperCollider:

```nim
import "functions.omni"

ins 3:
    "sig"
    "offset"
    "overdrive"

outs 1:
    "distorted_signal"

sample:
    offset = clip(in2, 0.0, 1.0)
    drive = clip(in3, 0.0, 1.0)
    pregain = pow(10.0, (2.0 * drive))
    x = clip(((in1 + offset) * pregain), -1.0, 1.0)
    out1 = x - (x * x * x) / 3.0
```

What is also really great about this project is the attention to inclusiveness and extensibility. Omni exists as its own project, while the wrappers that allow you to make objects or ugens for Max and SuperCollider respectively are their own separate entities. This means that you can develop your own wrappers or quickly tap into existing ones. VST and Open Frameworks wrapper anyone?

The links below take you to all the `omni` related things:

### [Omni](https://vitreo12.github.io)

### [Omni for SuperCollider](https://github.com/vitreo12/omnicollider)

### [Omni for Max](https://github.com/vitreo12/omnimax)

Thanks [Francesco](https://github.com/vitreo12)!



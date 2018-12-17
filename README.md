
# [How to Grow Neat Software Architecture out of Jupyter Notebooks](https://github.com/guillaume-chevalier/How-to-Grow-Neat-Software-Architecture-out-of-Jupyter-Notebooks)

> **Growing software out of your notebooks - the right way.**

Have you ever been in the situation where you've got Jupyter notebooks (iPython notebooks) so huge that you were feeling stuck in your code? Or even worse: have you ever found yourself duplicating your notebook to do changes, and then ending up with lots of badly named notebooks? Well, we've all been here if using notebooks long enough. So how should we code with notebooks? 

First, let's see why we need to be careful with notebooks. Then, let's see how to do TDD inside notebook cells and how to grow a neat software architecture out of your notebooks. We'll also discuss acceptance tests, unit tests, visualization tests, performance (fitting) tests, and tradeoffs to do if you want to keep your software clean when doing research and development, or research.


## Let's start with why.

Okay, so you like notebooks. Why so? Well... it feels easier you will say. Or perhaps that it's nice to see the output live while writing code and debugging it at the same time. One way to say it goes like this: it allows faster feedback in your test-code-refactor loop of [Test-Driven Development (TDD)](https://en.wikipedia.org/wiki/Test-driven_development). 

But wait. We are giving up on TDD when we write notebooks. This is because on the moment, you can have the feedback you would like to have during test-writing but in real time. This is really about debugging the application at the same time of writing it. Thinking about it, it's a very nice way to program. But it's easy to forget to write your tests, and it's easy to shoot yourself in the foot by writing notebooks too huge.


## How?

Okay, so we should keep using notebooks. But how to do that? Here is one solution: to grow your app out of your notebook. Yes, you'll first code the most basic things, and you'll need to **extract what you code to external python files you will import**. The easiest example I have of this is in [that machine learning project of mine](https://github.com/guillaume-chevalier/seq2seq-signal-prediction), it's possible to observe that I extracted the data loading functions to an external file. The notebook remains an exercise or tutorial, but for a more serious project, more logic would have ben extracted out of the notebook. 

And what about tests? Well, tests have three parts: first, you prepare it, then you execute it, and finally you assert that everything is as expected. Look at how you code when working with notebooks. Your code looks like a little intro, then a little interlude, and the finale. How to do your unit tests? Well, the intro should look most of the time like a test set-up. Maybe you're loading data from your external data loading function you already extracted. The little interlude is the logic you're currently implementing. You should make a class out of it, or at least something, such as mere functions. Then your finale mostly looks like the moment where you assert in your tests. When you're ready, create your tests out of those three parts of your notebook. Okay - this will be a big test. Much like an acceptance test, which is often called a functional test in the industry. This is normally the first test you write. Well, you've got it written here - it's only that it's not yet extracted to a test file. Your reflex might be to only extract the execution (interlude) of your notebook out to a function and move on. 

But a better thing to do is not only to do that, but also to then extract your test instead of "just scrapping" what you just coded in this notebook. Because let's face it: most of the time this setup would either change so much that in a few moments your notebook wouldn't look the same, or it would be thrown away. This is your test. It needs to be taken care of if you want your application to be stable. Extract a test before it's too late. 

Okay, so you understand the big picture on how to write viable software with notebooks. But if you're already doing a lot of TDD, you'll realize that the scenario I've just shown you is still flawed: you should write your test before! One of the reason why TDD is good is that it makes us think of the design up-front, which results in a [cleaner architecture](https://www.amazon.com/Clean-Architecture-Craftsmans-Software-Structure/dp/0134494164) and [cleaner code](https://www.amazon.ca/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) (not speaking of being a [clean coder](https://www.amazon.ca/Clean-Coder-Conduct-Professional-Programmers/dp/0137081073) yet). Let's rewind a bit. Now, it's easier to explain: once you've got the data, don't code yet the core of your logic at the middle of your notebook (which would be at the middle of your test). Code the assertion first. This will force you to think of the design up front: your assertion will need to call code that doesn't exist yet, so you'll start by creating the code from the perspective of the person using it! THAT - is TDD. Eureka. 


## What?

### What about the architecture?

There is more to building an app to extracting functions to external files. At some point, your application is going to grow from the grounds up. This is a nice moment to setup a layered architecture. For example, you could adhere to the principles of the [Domain Driven Design](https://martinfowler.com/tags/domain%20driven%20design.html) or of the [Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html). For example, I grew a service and domain layer out of notebooks when working recently on a [machine learning document clustering](https://github.com/ArtificiAI/Multilingual-Latent-Dirichlet-Allocation-LDA) service for a client at my [consulting company](http://www.neuraxio.com/en/). 

*Side note: I also grew some Machine-Learning-oriented layers in [a recent school project of mine](https://github.com/guillaume-chevalier/Sentiment-Classification-and-Language-Detection) (such as a pipeline layer and a pipeline object layer for the nice "[pipe and filter](https://docs.microsoft.com/en-us/azure/architecture/patterns/pipes-and-filters)" machine learning data pipeline pattern) - but note that no tests were made for the time being, given it was a school project that will probably only be useful for reference. One very prolific pattern that I've found for machine learning projects is the pipe and filter pattern as implemented in scikit-learn's [Pipeline](https://scikit-learn.org/stable/tutorial/statistical_inference/putting_together.html) object for which it's easy to add new pipeline elements as classes that inherit from some base pipeline element classes. It's easy to transform and to fit data in multiple consecutive steps with this pattern.*

Or maybe you're building a library like [NumPy](http://www.numpy.org/) and you need less of an architecture, but only a clear [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming) structure. 

Either and in any ways, for every medium to big application, you'll need to get out of notebooks at some point. It's easier to grow code from the grounds up (bottom-up) like this than top-down. To write code top-down would be very hard, if not impossible, within notebooks. 


### What about the visualization?

Once you extract things out of the notebooks, you might only need the notebook for visualization purposes. So you better have extracted everything to functions and classes so well that your notebook consists of a few lines and then your visualizations once refactored after extracting the tests. If you have too many visualizations, this might be the sign that you would have needed more notebooks instead of one big. And for the sake of having stable code and ensuring your visualizations still works without errors, you could probably extract those visualizations as "visualization tests" and be able to run those tests with your unit test runner (but "mute" the visual output when ran within the test suite). Here are some examples of visualization notebooks I've made to monitor and validate the performance of deep learning algorithms: 
- https://github.com/guillaume-chevalier/Linear-Attention-Recurrent-Neural-Network/blob/master/AnalyzeTestHyperoptResults_round_1.ipynb
- https://github.com/guillaume-chevalier/Linear-Attention-Recurrent-Neural-Network/blob/master/AnalyzeTestHyperoptResults_round_2.ipynb
- https://github.com/guillaume-chevalier/Hyperopt-Keras-CNN-CIFAR-100/blob/Vooban/AnalyzeTrainHyperoptResults.ipynb
- https://github.com/guillaume-chevalier/Hyperopt-Keras-CNN-CIFAR-100/blob/Vooban/AnalyzeTestHyperoptResults.ipynb


### What about the unit tests?

There is still one thing that we didn't talk about: the unit tests. You are (probably) so caught up with your quick development cycle that you forget them unit tests. What do you do instead? Code a block, run a cell, improve something. There it is - your TDD cycle. But it's in no way a proper way to do TDD because there, you didn't think of the design upfront. 

Well... those are the hardest kind of tests to deal with when working with notebooks. A good way to work would be to write your acceptance test first, and then a unit tests. From this first unit test and now on, start cycling through the coding-refactoring-testing unit testing loop. What happen if you start writing tests in your notebooks? The tests will stack at the end cells in chain and will somehow start to be burdensome until you extract everything out of the notebook. 


## Research, Practical Research, Research and Development: a note on clean code.

If you're working with notebooks, it is **highly likely** that you're doing research and development instead of plain old coding. You've got a tradeoff here. If doing research and development, to keep your amazing-10x-working-speed-multiplier, it might be a good idea to skip unit tests, but to then rewrite things once you're confident to put your discoveries together - now with tests. I know, this isn't ideal, but research and development is what it is. And if you're more on the side of research, you may skip the tests all along and only have some train/validation/test sets as your only tests, as performance (fitting) tests. It's well-known that research code is hard to reuse and is often very dirty. What alternatives would you suggest to keep things gracious when dealing with the unknown? Don't hesitate to write to me or to open an issue to give suggestions. 


## Conclusion

I hope you were satisfied by this reading. To sum up, you've learned how to grow a software architecture out of notebooks and how to properly do the TDD loop. It's important to write the tests before instead of after if you want to increase your chances of having a good software architecture. And at some point, the notebooks will be only used for visualization purposes, because everything will have been extracted to functions already. Once your software is finished, the only remaining use of your notebooks will be to continue adding new functionnalities, to visualize things, and to write tutorials (such as [here](https://github.com/guillaume-chevalier/Hyperopt-Keras-CNN-CIFAR-100/blob/Vooban/IntroductionToHyperopt.ipynb) and [here](https://github.com/Neuraxio/Multilingual-Latent-Dirichlet-Allocation-LDA/blob/master/Multilingual-LDA-Pipeline-Tutorial.ipynb)) on how your code works. With notebooks, however, doing unit tests remains hard. What would you do?.


## License

[CC-BY 4.0](https://creativecommons.org/licenses/by/4.0/)

Copyright (c) 2018 Guillaume Chevalier


## Extra links

### Connect with me

- [LinkedIn](https://ca.linkedin.com/in/chevalierg)
- [Twitter](https://twitter.com/guillaume_che)
- [GitHub](https://github.com/guillaume-chevalier/)
- [Quora](https://www.quora.com/profile/Guillaume-Chevalier-2)
- [YouTube](https://www.youtube.com/c/GuillaumeChevalier)
- [Dev/Consulting](http://www.neuraxio.com/en/)

### Liked this article? Did it help you? Leave a [star](https://github.com/guillaume-chevalier/How-to-grow-neat-software-architecture-out-of-jupyter-notebooks/stargazers), [fork](https://github.com/guillaume-chevalier/How-to-grow-neat-software-architecture-out-of-jupyter-notebooks/network/members) and share the love!

This article has been seen in:
- [HackerNews' 1st page](https://news.ycombinator.com/item?id=18661303)
- [Nat Torkington's ideas list, O'Reilly Media](https://www.oreilly.com/ideas/four-short-links-12-december-2018)
- [PyCoder's Weekly](https://pycoders.com/issues/346)
- And more.
